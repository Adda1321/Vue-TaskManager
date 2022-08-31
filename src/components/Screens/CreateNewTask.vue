


<script setup>
import { ref } from "vue";
defineProps({
  secondaryButton: String,
  primaryButton: String,
  inputStyle: String,
  cardStyle: String,
});

import Datepicker from "@vuepic/vue-datepicker";
import "@vuepic/vue-datepicker/dist/main.css";

const description = ref("");
const title = ref("");
const date = ref();
const format = (date) => {
  const day = date.getDate();
  const month = date.toLocaleString('default', { month: 'long' });
  const year = date.getFullYear();
console.log('FORMAT DATAA',`${day}/${month}/${year}`)
  return `${month}/${day}/${year}`;
};
const emit = defineEmits(["cancelTask", "submitTask"]);
</script>
<template>
  <div
    :class="cardStyle"
    class="border-l-0 h-60 vflex justify-between"
    style="width: 600px"
  >
    <div>
      <input
        v-model="title"
        placeholder="enter title"
        type="text"
        :class="inputStyle"
        required
      />
      <br />
      <input
        v-model="description"
        placeholder="enter task description"
        :class="inputStyle"
        required
      />
    </div>
    <div class="mt-2">
      <span>Due Date:</span>
      <Datepicker
        v-model="date"
        class="inline-block ml-2"
        :format="format"
      ></Datepicker>
    </div>
    <div class="mt-4">
      <button
        :disabled="
          title.replace(/\s/g, '').length < 1 ||
          description.replace(/\s/g, '').length < 1
        "
        :class="primaryButton"
        @click="emit('submitTask', title, description , format(date))"
      >
        Create Task
      </button>

      <button :class="secondaryButton" @click="emit('CancelTask')">
        Cancel task
      </button>
   
    </div>
  </div>
</template>
<style scoped>
</style>