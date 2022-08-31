<template>
  <div class="bg-emerald-600 ">
     
 <AllTasks></AllTasks>

  </div>
</template>

<script>
import AllTasks from './components/Screens/AllTasks.vue';

export default {
  name: "App",
  components: {
    AllTasks
},

};
</script>

<style scoped>
* { 
  height: 100vh;
}
</style>
