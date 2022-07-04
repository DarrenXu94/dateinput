

<template>
    <div>
        <label for="BDay01" id="BDay01Label">
            Birth date
            <span>(MM/DD/YYYY)</span>:
        </label>
        <input
            type="text"
            id="BDay01"
            aria-labelledby="BDay01Label BDay01error"
            aria-describedby="BDay01confirm"
            @blur="valiDate('BDay01', 'BDay01confirm', 'BDay01error')"
        />
        <output class="confirm" id="BDay01confirm" aria-live="assertive" for="BDay01" form="Form"></output>
        <div class="error" id="BDay01error"></div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
    emits: ['update:date'],

    data() {
        return {
            message: 'Hello World!'
        }
    },
    methods: {
        valiDate(fieldID: string, confirmID: string, errorID: string) {
            var field = document.getElementById(fieldID) as HTMLInputElement;
            var confirm = document.getElementById(confirmID) as HTMLElement;
            var error = document.getElementById(errorID) as HTMLElement;
            var d = field.value;

            if (!isNaN(Date.parse(d))) {
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
        }
    }
})

</script>

<style>
</style>
