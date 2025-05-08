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
  import { ref, onMounted } from 'vue';
  
  const newTask = ref('');
  const tasks = ref([]);
  
  // Загрузка задач из localStorage
  onMounted(() => {
    const savedTasks = localStorage.getItem('todo-tasks');
    if (savedTasks) {
      tasks.value = JSON.parse(savedTasks);
    }
  });
  
  // Добавление новой задачи
  const addTask = () => {
    if (newTask.value.trim()) {
      tasks.value.push({
        text: newTask.value.trim(),
        completed: false
      });
      newTask.value = '';
      saveTasks();
    }
  };
  
  // Удаление задачи
  const removeTask = (index) => {
    tasks.value.splice(index, 1);
    saveTasks();
  };
  
  // Сохранение задач в localStorage
  const saveTasks = () => {
    localStorage.setItem('todo-tasks', JSON.stringify(tasks.value));
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