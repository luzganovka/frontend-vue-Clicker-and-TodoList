<template>
    <div class="clicker-container">    
      <button @click="click" class="clicker-button">Click</button>
      <p class="click-count">Clicked {{ count }} times</p>
    </div>
  </template>
  
  <script setup>
  import { ref, defineProps, defineEmits } from 'vue';
  
  const props = defineProps({
      amount: {
          type: Number,
          default: 1
      }
  });
  
  const count = ref(0);
  const event = defineEmits(['button-clicked']);
  
  const click = () => {
      count.value += props.amount; 
      event('button-clicked'); 
  };
  </script>
  
  <style scoped>
  .clicker-container {
    padding: 15px;
    border-radius: 8px;
    background-color: #f8f9fa;
    /* transition: transform 0.2s; */
  }
  
  .clicker-container:hover {
    transform: translateY(-2px);
  }
  
  .clicker-button {
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
    background-color: #42b983;
    color: white;
    border: none;
    border-radius: 4px;
    /* transition: background-color 0.3s; */
  }
  
  .clicker-button:hover {
    background-color: #3aa876;
  }
  
  .click-count {
    margin-top: 10px;
    font-size: 14px;
    color: #666;
  }
  </style>