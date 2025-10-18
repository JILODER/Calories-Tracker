<script setup lang="ts">
import CaloriesForm from './components/CaloriesForm.vue'
</script>

<template>
  <main>
    <h1>Calories Tracker </h1>
    <CaloriesForm />
  </main>
</template>

<style scoped>
main {
  text-align: center;
  padding: 2rem;
  background-color: #f4f7f6;
  min-height: 100vh;
}

h1 {
  margin-bottom: 1.5rem;
  color: #333;
}
</style>
  