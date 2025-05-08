<template>
    <div class="todo-app">
      <h1>ToDo List</h1>
      <div class="add-task">
        <input 
          v-model="newTask" 
          @keyup.enter="addTask" 
          placeholder="New Task"
        >
        <button @click="addTask">Add</button>
      </div>
      
      <div class="task-list">
        <div 
          v-for="(task, index) in tasks" 
          :key="index" 
          class="task-item"
        >
          <input 
            type="checkbox" 
            v-model="task.completed"
          >
          <span :class="{ completed: task.completed }">
            {{ task.text }}
          </span>
          <button @click="removeTask(index)">×</button>
        </div>
      </div>
    </div>
  </template>
  
  
  <script setup>
  import { ref, onMounted, watch } from 'vue';
  
  const STORAGE_KEY = 'vue-todo-list';
  
  const newTask = ref('');
  const tasks = ref([]);
  
  // Загрузка задач при монтировании компонента
  onMounted(() => {
    const savedTasks = localStorage.getItem(STORAGE_KEY);
    if (savedTasks) {
      tasks.value = JSON.parse(savedTasks);
    }
  });
  
  // Автоматическое сохранение при изменении задач
  watch(tasks, (newTasks) => {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(newTasks));
  }, { deep: true });
  
  const addTask = () => {
    if (newTask.value.trim()) {
      tasks.value.push({
        id: Date.now(),
        text: newTask.value.trim(),
        completed: false,
        createdAt: new Date().toISOString()
      });
      newTask.value = '';
    }
  };
  
  const removeTask = (id) => {
    tasks.value = tasks.value.filter(task => task.id !== id);
  };
  </script>
  


  <style scoped>
  .todo-app {
    max-width: 500px;
    margin: 0 auto;
    padding: 20px;
    font-family: Arial, sans-serif;
  }
  
  h1 {
    text-align: center;
    color: #333;
  }
  
  .add-task {
    display: flex;
    margin-bottom: 20px;
  }
  
  .add-task input {
    flex-grow: 1;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px 0 0 4px;
  }
  
  .add-task button {
    padding: 10px 20px;
    background-color: #42b983;
    color: white;
    border: none;
    border-radius: 0 4px 4px 0;
    cursor: pointer;
  }
  
  .task-list {
    border-top: 1px solid #ccc;
    padding-top: 10px;
  }
  
  .task-item {
    display: flex;
    align-items: center;
    padding: 10px;
    color: #444;
    border-bottom: 1px solid #ccc;
  }
  
  .task-item input[type="checkbox"] {
    margin-right: 10px;
  }
  
  .task-item span {
    flex-grow: 1;
  }
  
  .task-item span.completed {
    text-decoration: line-through;
    color: #888;
  }
  
  .task-item button {
    background: none;
    border: none;
    color: #ff4444;
    font-size: 1.2em;
    cursor: pointer;
  }
  </style>