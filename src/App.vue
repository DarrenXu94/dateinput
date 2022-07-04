<script lang="ts">
import DateInput from "./components/DateInput.vue";
import { defineComponent } from "vue";
export default defineComponent({
  components: { DateInput },
  methods: {
    updateDate(date: Date) {
      console.log('date', date)
    }
  }
})
</script>

<template>
  <DateInput @update:date="updateDate" />
</template>

<style>
</style>
