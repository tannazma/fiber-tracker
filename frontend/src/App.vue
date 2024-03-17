<template>
  <nav>
    <!-- <router-link to="/">Home</router-link> | -->
    <!-- <router-link to="/about">About</router-link> -->
  </nav>
  <div>
    <h1>Fiber Tracker</h1>
    <input type="text" v-model="searchQuery" placeholder="brown bread" />
    <div v-for="(fiberFood, index) in filteredFiberFoods" :key="index">
      <h2>{{ fiberFood.name }}</h2>
      <img :src="fiberFood.image" alt="fiberFood.name" :width="150" />
      <p>Instruction: {{ fiberFood.instruction }}</p>
      <p>Score: {{ fiberFood.amount }}</p>
    </div>
  </div>
  <router-view />
</template>

<script setup>
import { ref, onMounted, computed } from "vue";
const fiberFoods = ref([]);
const searchQuery = ref("");

onMounted(async () => {
  const response = await fetch("/data/data.json");
  if (!response.ok) {
    throw new Error("Failed to fetch data)");
  }
  const jsonData = await response.json();
  fiberFoods.value = jsonData;
});

const filteredFiberFoods = computed(() => {
  const query = searchQuery.value.toLowerCase();
  return fiberFoods.value.filter((fiberFood) =>
    fiberFood.name.toLowerCase().includes(query)
  );
});
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
