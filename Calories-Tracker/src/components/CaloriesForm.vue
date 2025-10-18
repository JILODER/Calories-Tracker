<template>
  <div class="form-container">
    <h2>Agregar comida </h2>

    <form @submit.prevent="addMeal">
      <div class="input-group">
        <label for="name">Nombre:</label>
        <input
          id="name"
          v-model="mealName"
          type="text"
          placeholder="Ej. Manzana"
          required
        />
      </div>

      <div class="input-group">
        <label for="calories">Calorías:</label>
        <input
          id="calories"
          v-model.number="mealCalories"
          type="number"
          min="0"
          placeholder="Ej. 95"
          required
        />
      </div>

      <button type="submit">Agregar</button>
    </form>

    <ul class="meals-list">
      <li v-for="(meal, index) in meals" :key="index">
        {{ meal.name }} — <strong>{{ meal.calories }} kcal</strong>
      </li>
    </ul>

    <p v-if="meals.length > 0" class="total">
      Total: <strong>{{ totalCalories }}</strong> kcal
    </p>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, watch, onMounted } from 'vue'

const mealName = ref('')
const mealCalories = ref<number | null>(null)
const meals = ref<{ name: string; calories: number }[]>([])

// 🔹 Cargar comidas guardadas al iniciar
onMounted(() => {
  const storedMeals = localStorage.getItem('meals')
  if (storedMeals) {
    meals.value = JSON.parse(storedMeals)
  }
})

// 🔹 Guardar automáticamente cuando cambian las comidas
watch(meals, (newMeals) => {
  localStorage.setItem('meals', JSON.stringify(newMeals))
}, { deep: true })

function addMeal() {
  if (mealName.value && mealCalories.value) {
    meals.value.push({
      name: mealName.value,
      calories: mealCalories.value,
    })
    mealName.value = ''
    mealCalories.value = null
  }
}

const totalCalories = computed(() =>
  meals.value.reduce((acc, meal) => acc + meal.calories, 0)
)
</script>


<style scoped>
.form-container {
  max-width: 400px;
  margin: 2rem auto;
  background: #fdfdfd;
  padding: 1.5rem;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  color: #222; /* texto más oscuro */
  font-family: "Inter", sans-serif;
}

h2 {
  text-align: center;
  margin-bottom: 1rem;
  color:#050505 ;
}

.input-group {
  margin-bottom: 1rem;
}

label {
  display: block;
  font-weight: 600;
  margin-bottom: 0.25rem;
}

input {
  width: 100%;
  padding: 0.5rem;
  border-radius: 8px;
  border: 1px solid #0f0f0f;
  font-size: 1rem;
}

button {
  display: block;
  width: 100%;
  background-color: #42b883;
  color: rgb(236, 234, 234);
  border: none;
  padding: 0.75rem;
  border-radius: 8px;
  font-size: 1rem;
  cursor: pointer;
}

button:hover {
  background-color: #369870;
}

.meals-list {
  list-style: none;
  padding: 0;
  margin-top: 1.5rem;
}

.meals-list li {
  padding: 0.5rem 0;
  border-bottom: 1px solid #050505;
}

.total {
  margin-top: 1rem;
  text-align: center;
  font-size: 1.2rem;
  font-weight: bold;
}
</style>
