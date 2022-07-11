<template>
  <div :key="task.id" v-for="task in tasks">
    <h3>{{ task.text }}</h3>
  </div>
</template>

<script>
export default {
  name: 'Task',
  props: {
    tasks: Array,
  },
};
</script>

<style scoped>

</style>