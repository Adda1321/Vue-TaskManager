


<script setup>
import { ref, onMounted } from "vue";

import Datepicker from "@vuepic/vue-datepicker";
import "@vuepic/vue-datepicker/dist/main.css";

const props = defineProps([
  "index",
  "task",
  "secondaryButton",
  "primaryButton",
  "inputStyle",
]);

let title = ref("");
let description = ref("");
const date = ref("");
const format = (date) => {
  console.log("DATEEE", date);
  const day = date.getDate();
  const month = date.toLocaleString("default", { month: "long" });
  const year = date.getFullYear();
  return `${month}/${day}/${year}`;
};

onMounted(() => {
  date.value = props.task.dueDate;
  title.value = props.task.title;
  description.value = props.task.description;
});

const emit = defineEmits(["cancelTask", "submitTask", "submitEditTask"]);
</script>
<template>
  <div style="width: 500px">
    <input
      v-model="title"
      placeholder="enter title"
      :class="props.inputStyle"
      required
    />
    <br />
    <input
      v-model="description"
      placeholder="enter task description"
      :class="props.inputStyle"
      required
    />
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
        :class="props.primaryButton"
        @click.prevent="
          emit(
            'submitEditTask',
            title,
            description,
            props.task.isComplete,
            props.index,
            format(new Date (date))
          )
        "
      >
        Edit Task
      </button>

      <button :class="props.secondaryButton" @click="emit('CancelTask')">
        Cancel task
      </button>
    </div>
  </div>
</template>
<style scoped>
</style>