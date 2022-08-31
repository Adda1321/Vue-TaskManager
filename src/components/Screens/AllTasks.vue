<template>
  <div class="flex flex-col items-center justify-center">
    <div class="text-black tracking-wide font-medium cursor-pointer mb-2 mt-28">
      <a class="hover:text-cyan-500 hover:underline hover: underline-offset-4">
        All Tasks /
      </a>
      <a
        class="hover:text-cyan-500 hover:underline hover: underline-offset-4"
        @click="(newTask = true), (editTask = null)"
      >
        New Task +</a
      >
    </div>
    <div v-if="newTask">
      <CreateNewTask
        :primaryButton="primaryButton"
        :secondaryButton="secondaryButton"
        :inputStyle="inputStyle"
        :cardStyle="cardStyle"
        @cancelTask="CancelTask"
        @submitTask="submitTask"
      ></CreateNewTask>
    </div>

    <div class="overflow-y-scroll scrollbar-hide">
      <div v-for="(task, index) in tasks" :key="index">
        <div
          class="
            block p-2 py-4 mb-6 w-auto h-auto bg-white rounded-lg shadow-mdhover:bg-gray-100 dark:bg-gray-800 dark:border-gray-700 dark:hover:bg-gray-700 border-l-8
          "
          :class="{
            'border-green-500': task.isComplete,
            'border-red-500': !task.isComplete,
          }"
        >
          <div class="">
            <div v-if="editTask === index">
              <EditTask
                :index="index"
                :task="task"
                :primaryButton="primaryButton"
                :inputStyle="inputStyle"
                :secondaryButton="secondaryButton"
                @cancelTask="CancelTask"
                @submitEditTask="submitEditTask"
              ></EditTask>
            </div>

            <div v-else-if="editTask !== index" @click="ShowDetails(index)">
              <div class="flex justify-between w-80 h-10">
                <h2 class="font-medium">
                  {{ task.title }}
                </h2>

                <div class="flex justify-between items-center w-20">
                  <div @click="handleCompleteTask(index)" :title="complete">
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 448 512"
                      class="w-4 h-4 fill-cyan-500 hover:fill-green-400"
                    >
                      <path
                        d="M438.6 105.4C451.1 117.9 451.1 138.1 438.6 150.6L182.6 406.6C170.1 419.1 149.9 419.1 137.4 406.6L9.372 278.6C-3.124 266.1-3.124 245.9 9.372 233.4C21.87 220.9 42.13 220.9 54.63 233.4L159.1 338.7L393.4 105.4C405.9 92.88 426.1 92.88 438.6 105.4H438.6z"
                      />
                    </svg>
                  </div>

                  <div @click="handleEdit(task, index)">
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 512 512"
                      class="w-3 h-3 fill-cyan-500 hover:fill-cyan-700"
                    >
                      <path
                        d="M362.7 19.32C387.7-5.678 428.3-5.678 453.3 19.32L492.7 58.75C517.7 83.74 517.7 124.3 492.7 149.3L444.3 197.7L314.3 67.72L362.7 19.32zM421.7 220.3L188.5 453.4C178.1 463.8 165.2 471.5 151.1 475.6L30.77 511C22.35 513.5 13.24 511.2 7.03 504.1C.8198 498.8-1.502 489.7 .976 481.2L36.37 360.9C40.53 346.8 48.16 333.9 58.57 323.5L291.7 90.34L421.7 220.3z"
                      />
                    </svg>
                  </div>

                  <div @click="handleDelete(index)">
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 448 512"
                      class="w-3 h-3 fill-cyan-500 hover:fill-red-700"
                    >
                      <path
                        d="M135.2 17.69C140.6 6.848 151.7 0 163.8 0H284.2C296.3 0 307.4 6.848 312.8 17.69L320 32H416C433.7 32 448 46.33 448 64C448 81.67 433.7 96 416 96H32C14.33 96 0 81.67 0 64C0 46.33 14.33 32 32 32H128L135.2 17.69zM394.8 466.1C393.2 492.3 372.3 512 346.9 512H101.1C75.75 512 54.77 492.3 53.19 466.1L31.1 128H416L394.8 466.1z"
                      />
                    </svg>
                  </div>
                </div>
              </div>
              <div class="flex justify-between">
                <div v-show="task.selected" class="w-56">
                  {{ task.description }}
                </div>
                <h3 class="text-rose-700">{{task.dueDate}}</h3>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import CreateNewTask from './CreateNewTask.vue';
import EditTask from "./EditTask.vue";
const cardStyle = ref(
  " block p-2 py-4 mb-6 w-80 h-20 bg-white rounded-lg shadow-md hover:bg-gray-100 dark:bg-gray-800 dark:border-gray-700 dark:hover:bg-gray-700 border-l-8 "
);

const inputStyle = ref(
  "bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
);

const primaryButton = ref(
  "bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-2 mx-2 rounded "
);
const secondaryButton = ref(
  " bg-transparent hover:bg-blue-500 text-blue-700 font-semibold hover:text-white py-2 px-4 border border-blue-500 hover:border-transparent rounded "
);

const tasks = ref([
  {
    title: "Demo Title",
    description: "Please create demo description ..",
    selected: null,
    isComplete: false,
    dueDate:'August/2/2022'
    
  },
]);

let editTask = ref(null);

let newTask = ref(false);

const ShowDetails = (index) => {
  tasks.value[index].selected = !tasks.value[index].selected;
};
const submitTask = (Title, Description , format) => {

  editTask.value = false;
  tasks.value.unshift({
    title: Title,
    description: Description,
    selected: null,
    isComplete: false,
    dueDate:format
  });
  newTask.value = false;
};

const submitEditTask = (Title, Description, isComplete, index , format) => {
 
newTask.value = false;
  tasks.value[index] = {
    title: Title,
    description: Description,
    selected: null,
    isComplete: isComplete,
    dueDate: format
  };
  editTask.value = false;
};
const handleEdit = (task, index) => {
  editTask.value = index;
  newTask.value = false;
};
const handleCompleteTask = (index) => {
  tasks.value[index].isComplete = !tasks.value[index].isComplete;
};
const handleDelete = (index) => {
  tasks.value.splice(index, 1);
};
const CancelTask = () => {
  newTask.value = false;
  editTask.value = null;
};
</script>

<style scoped>
</style>