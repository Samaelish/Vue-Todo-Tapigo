<template>
  <div class="app">
    <h1>Список задач</h1>
    <div v-for="task in tasks" :key="task.id" class="task-item">
      <label>
        <input type="checkbox" v-model="task.done" @change="saveTasks">
        <span :class="{ completed: task.done }">{{ task.title }}</span>
      </label>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: []
    }
  },
  async mounted() {
    const savedTasks = localStorage.getItem('tasks');

    if (savedTasks) {
      this.tasks = JSON.parse(savedTasks);
    } else {
      const response = await fetch('/tasks.json');
      this.tasks = await response.json();
      this.saveTasks();
    }
  },
  methods: {
    saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    }
  }
}
</script>

<style>
.app {
  max-width: 600px;
  margin: 20px auto;
  padding: 20px;
}

.task-item {
  padding: 10px;
  border-bottom: 1px solid #eee;
}

.completed {
  text-decoration: line-through;
  color: #888;
}

input[type="checkbox"] {
  margin-right: 10px;
}
</style>
