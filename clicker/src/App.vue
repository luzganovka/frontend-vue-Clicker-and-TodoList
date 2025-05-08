<template>
  <div id="app">
    <h1>100 Clickers</h1>
    <div class="clickers-grid">
      <div v-for="n in 100" :key="n" class="clicker-item">
        <Clicker :amount="1" @button-clicked="click(n)">
          Click
        </Clicker>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineEmits } from 'vue';
import Clicker from './components/clicker.vue';

const emit = defineEmits();

const click = (n) => {
  alert(`Clicker ${n} clicked!`);
};
</script>

<style scoped>
#app {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
}

.clickers-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
  margin-top: 30px;
}

.clicker-item {
  display: flex;
  justify-content: center;
}

</style>