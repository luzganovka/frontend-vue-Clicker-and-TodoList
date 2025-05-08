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
        v-for="task in tasks" 
        :key="task.id" 
        class="task-item"
      >
        <input 
          type="checkbox" 
          v-model="task.completed"
          @change="toggleTask(task)"
        >
        <span :class="{ completed: task.completed }">
          {{ task.title }}
        </span>
        <button @click="removeTask(task.id)">×</button>
      </div>
    </div>
  </div>

  <Popup :isOpen="showDeletePopup" @confirm="confirmDelete" @cancel="cancelDelete">
    <h3>Delete?</h3>
    <p>Please ensure and then confirm!</p>
    <div class="actions">
      <button class="cancel-btn" @click="cancelDelete">No, cancel!</button>
      <button class="confirm-btn" @click="confirmDelete">Yes, delete it!</button>
    </div>
  </Popup>
</template>



<script setup>
import { ref, onMounted } from 'vue';
import Popup from './Popup.vue';

const showDeletePopup = ref(false);
const taskToDelete = ref(null);

const removeTask = (id) => {
  taskToDelete.value = id;
  showDeletePopup.value = true;
};

const confirmDelete = async () => {
  try {
    await fetch(`${API_URL}/${taskToDelete.value}`, {
      method: 'DELETE'
    });
    tasks.value = tasks.value.filter(task => task.id !== taskToDelete.value);
    showDeletePopup.value = false;
    taskToDelete.value = null;
  } catch (error) {
    console.error('Ошибка удаления задачи:', error);
    showDeletePopup.value = false;
  }
};

const cancelDelete = () => {
  showDeletePopup.value = false;
  taskToDelete.value = null;
};


const API_URL = 'https://jsonplaceholder.typicode.com/todos';
const USER_ID = 1;

const newTask = ref('');
const tasks = ref([]);

// Загрузка задач
const fetchTasks = async () => {
  try {
    const response = await fetch(`${API_URL}?userId=${USER_ID}`);
    tasks.value = await response.json();
  } catch (error) {
    console.error('Ошибка загрузки задач:', error);
  }
};

// Добавление задачи
const addTask = async () => {
  if (!newTask.value.trim()) return;
  
  try {
    const response = await fetch(API_URL, {
      method: 'POST',
      body: JSON.stringify({
        userId: USER_ID,
        title: newTask.value.trim(),
        completed: false
      }),
      headers: {
        'Content-type': 'application/json; charset=UTF-8',
      },
    });
    
    const newTaskData = await response.json();
    tasks.value.unshift({ ...newTaskData, id: Date.now() }); // Фейковый API возвращает id=201 всегда
    newTask.value = '';
  } catch (error) {
    console.error('Ошибка добавления задачи:', error);
  }
};

// Изменение статуса
const toggleTask = async (task) => {
  try {
    const response = await fetch(`${API_URL}/${task.id}`, {
      method: 'PATCH',
      body: JSON.stringify({
        userId: USER_ID,
        completed: task.completed
      }),
      headers: {
        'Content-type': 'application/json; charset=UTF-8',
      },
    });
    
    const newTaskData = await response.json();

  } catch (error) {
    console.error('Ошибка обновления задачи:', error);
    task.completed = !task.completed; // Откатываем изменение при ошибке
  }
};


// Загружаем задачи при монтировании
onMounted(fetchTasks);
</script>
  


  <style scoped>
  .todo-app {
    max-width: 500px;
    margin: 0 auto;
    padding: 20px;
    font-family: Arial, sans-serif;
  }
  
  p {
    color: #333;
  }

  h1 {
    text-align: center;
    color: #333;
  }

  h3 {
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