<script lang="ts">
import DateInput from "./components/DateInput.vue";
import { defineComponent } from "vue";
export default defineComponent({
  components: { DateInput },
  methods: {
    updateDate(date: Date) {
      console.log('date', date)
    }
  },
  data() {
    return {
      value: new Date
    }
  },
})
</script>

<template>
  <h2>Provider</h2>
  <DateInput @update:date="updateDate" :initial-date="value" />
</template>

<style>
</style>
