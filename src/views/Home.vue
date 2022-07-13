<template>
  <div v-show="showAddTask">
    <AddTask @add-task="addTask"/>
  </div>
  <Tasks
    @toggle-reminder="toggleReminder"
    @delete-task="deleteTask"
    :tasks="tasks"
  />
</template>

<script>
import Tasks from '../components/Tasks';
import AddTask from '../components/AddTask';

export default {
  name: 'Home',
  props: {
    showAddTask: {
      type: Boolean,
    },
  },
  components: {
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
    };
  },
  methods: {
    async addTask(task) {
      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(task),
      });

      const newTask = await res.json();

      this.tasks = [...this.tasks, newTask];
    },
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id);
      const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(updTask),
      });

      const updatedTask = await res.json();

      this.tasks = this.tasks.map((task) => task.id === id ? updatedTask : task);
    },
    async deleteTask(id) {
      if (confirm('Are you sure?')) {
        const res = await fetch(`api/tasks/${id}`, {
          method: 'DELETE',
        });

        res.status === 200
          ? this.tasks = this.tasks.filter((task) => task.id !== id)
          : alert('Error deleting task');
      }
    },
    async fetchTasks() {
      const res = await fetch('api/tasks');
      return res.json();
    },
    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`);
      return res.json();
    },
  },
  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>

<style scoped>

</style>
