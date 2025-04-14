<template>
  <!-- Основной контейнер приложения -->
  <div class="max-w-[600px] mx-auto my-5 p-5">
    <!-- Заголовок приложения -->
    <h1 class="text-2xl font-bold mb-4 text-gray-800">Список задач</h1>

    <!-- Цикл для отрисовки задач -->
    <div v-for="task in tasks" :key="task.id" class="py-2.5 border-b border-gray-200">
      <!-- Элемент label для чекбокса -->
      <label class="flex items-center space-x-3 cursor-pointer">
        <!-- Чекбокс для отметки выполнения -->
        <input type="checkbox" v-model="task.done" @change="saveTasks"
          class="w-5 h-5 rounded border-gray-300 text-blue-600 focus:ring-blue-500 transform scale-125">
        <!-- Текст задачи с условным стилем -->
        <span :class="{
          'text-gray-400 line-through': task.done,
          'text-gray-700': !task.done
        }" class="text-lg transition-all duration-200">
          {{ task.title }}
        </span>
      </label>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // Хранилище задач
      tasks: []
    }
  },

  // Хук жизненного цикла - вызывается после монтирования компонента
  async mounted() {
    // Пытаемся получить сохраненные задачи из localStorage
    const savedTasks = localStorage.getItem('tasks')

    if (savedTasks) {
      // Если есть сохраненные задачи - используем их
      this.tasks = JSON.parse(savedTasks)
    } else {
      // Если нет - загружаем из JSON-файла
      const response = await fetch('/tasks.json')
      this.tasks = await response.json()
      // Сохраняем начальное состояние
      this.saveTasks()
    }
  },

  methods: {
    // Метод для сохранения задач в localStorage
    saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    }
  }
}
</script>

<style>
/* Основные стили контейнера */
.app {
  max-width: 600px;
  margin: 20px auto;
  /* Центрирование по горизонтали */
  padding: 20px;
}

/* Стили элемента задачи */
.task-item {
  padding: 10px;
  border-bottom: 1px solid #eee;
  /* Разделитель между задачами */
}

/* Стиль для выполненной задачи */
.completed {
  text-decoration: line-through;
  /* Перечеркнутый текст */
  color: #888;
  /* Серый цвет для выполненного */
}

/* Стилизация чекбокса */
input[type="checkbox"] {
  margin-right: 10px;
  /* Отступ от текста */
  transform: scale(1.2);
  /* Увеличение размера */
  cursor: pointer;
  /* Курсор-указатель */
}
</style>