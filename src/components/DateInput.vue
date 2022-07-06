

<template>
    <div>
        <label for="BDay01" id="BDay01Label">
            Birth date
            <span>(MM/DD/YYYY)</span>:
        </label>
        <input
            :value="value"
            type="text"
            id="BDay01"
            ref="inputElement"
            aria-labelledby="BDay01Label BDay01error"
            aria-describedby="BDay01confirm"
            @blur="valiDate"
            v-on:keydown.up.prevent="handleArrows"
            v-on:keydown.down.prevent="handleArrows"
            v-on:keydown.left="handleArrows"
            v-on:keydown.right="handleArrows"
        />
        <output
            class="confirm"
            id="BDay01confirm"
            ref="confirm"
            aria-live="assertive"
            for="BDay01"
            form="Form"
        ></output>
        <div ref="error" class="error" id="BDay01error"></div>
    </div>
</template>

<script lang="ts">
import dayjs, { type ManipulateType } from 'dayjs'

import { defineComponent } from "vue";
export default defineComponent({
    emits: ['update:date'],
    props: {
        initialDate: {
            type: [Date, String]
        }
    },
    mounted() {
        if (this.initialDate instanceof String) {
            this.value = this.initialDate as string;
        } else if (this.initialDate instanceof Date) {
            this.value = dayjs(this.initialDate).format('MM/DD/YYYY')
        }
    },
    data() {
        return {
            value: ''
        }
    },
    methods: {
        valiDate() {
            const field = this.$refs.inputElement as HTMLInputElement;
            const confirm = this.$refs.confirm as HTMLElement;
            let error = this.$refs.error as HTMLElement;
            let fieldValue = field.value;

            if (!isNaN(Date.parse(fieldValue))) {
                error.innerText = "";
                var objDate = new Date(field.value);
                var theYear = objDate.getFullYear();
                var theMonth = objDate.getMonth() as any;
                let locale = "en-us";

                theMonth = objDate.toLocaleString(locale, { month: "long" })
                var theDay = objDate.getDate();
                confirm.innerText =
                    "You entered " + theMonth + " " + theDay + ", " + theYear + "."
                field.setAttribute("aria-invalid", "false");
                this.$emit('update:date', objDate)

            } else {
                // alert('blank');
                confirm.innerText = "";
                error.innerText = "Please enter a date.";
                field.setAttribute("aria-invalid", "true");
            }
        },
        handleCursorJump(keyPressed: string, start: number, end: number, type: ManipulateType, index: number) {
            const field = this.$refs.inputElement as HTMLInputElement;

            if (keyPressed == 'ArrowRight') {
                field.selectionEnd = end
            }
            if (keyPressed == 'ArrowLeft') {
                field.selectionEnd = start
            }

            if (keyPressed == 'ArrowUp') {
                const date = dayjs(this.value).add(1, type)
                console.log(date)
                this.value = date.format('MM/DD/YYYY')
                this.$nextTick(() => {
                    field.selectionEnd = index
                })
            }
            if (keyPressed == 'ArrowDown') {
                const date = dayjs(this.value).subtract(1, type)
                this.value = date.format('MM/DD/YYYY')
                this.$nextTick(() => {
                    field.selectionEnd = index
                })
            }
        },
        handleArrows(evt: KeyboardEvent) {
            console.log(evt)
            // 0M1M2/3D4D5/6Y7Y8Y9Y10
            const keyPressed = evt.key;
            const index = (evt.target as any).selectionStart
            if (index >= 0 && index < 3) {
                this.handleCursorJump(keyPressed, 0, 2, 'month', index)
            } else if (index >= 3 && index <= 5) {
                this.handleCursorJump(keyPressed, 3, 5, 'day', index)
            } else {
                this.handleCursorJump(keyPressed, 6, 10, 'year', index)
            }
        }
    }
})

</script>

<style>
</style>
