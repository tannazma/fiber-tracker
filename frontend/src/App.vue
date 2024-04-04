<template>
  <div>
    <nav>
      <!-- <router-link to="/">Home</router-link> | -->
      <!-- <router-link to="/about">About</router-link> -->
    </nav>
    <h1>Fiber Tracker</h1>
    <input type="text" v-model="searchQuery" placeholder="brown bread" />
    <button @click="submit">+</button>
    <select v-model="selected" ref="selectMenu">
      <option
        v-for="(option, index) in options"
        :value="option.value"
        :key="index"
      >
        {{ option.text }}
      </option>
    </select>
    <div class="fiber-foods-container">
      <div
        v-for="(fiberFood, index) in filteredFiberFoods"
        :key="index"
        class="fiber-food-card"
      >
        <el-card>
          <template #header>
            <h2>{{ fiberFood.name }}</h2>
          </template>
          <img
            :src="imageBaseUrl + fiberFood.image"
            alt="fiberFood.name"
            :width="150"
          />
          <p>Instruction: {{ fiberFood.instruction }}</p>
          <p>Score: {{ fiberFood.amount }}</p>
        </el-card>
      </div>
    </div>
    <router-view />
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from "vue";
const fiberFoods = ref([]);
const searchQuery = ref("");
const selected = ref("A");
const selectMenu = ref(null);
const options = ref([
  { text: "One", value: "A" },
  { text: "Two", value: "B" },
  { text: "Three", value: "C" },
]);
const imageBaseUrl =
  process.env.NODE_ENV === "production" ? "/fiber-tracker/" : "";

onMounted(async () => {
  if (process.env.NODE_ENV !== "production") {
    const response = await fetch("/data/data.json");
    if (!response.ok) {
      throw new Error("Failed to fetch data)");
    }
    const jsonData = await response.json();
    fiberFoods.value = jsonData;
  } else if (process.env.NODE_ENV === "production") {
    const response = await fetch("/fiber-tracker/data/data.json");
    if (!response.ok) {
      throw new Error("Failed to fetch data)");
    }
    const jsonData = await response.json();
    fiberFoods.value = jsonData;
  }
});

const filteredFiberFoods = computed(() => {
  const query = searchQuery.value.toLowerCase();
  return fiberFoods.value.filter((fiberFood) =>
    fiberFood.name.toLowerCase().includes(query)
  );
});

const submit = () => {
  if (selectMenu.value) {
    selectMenu.value.focus();
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.fiber-foods-container {
  padding-top: 20px;
  display: flex;
  flex-wrap: wrap;
  align-items: flex-start;
  justify-content: space-between;
  gap: 20px;
}

.fiber-food-card {
  flex: 0 0 calc(20% - 20px);
  max-width: calc(20% - 20px);
}

@media (max-width: 968px) {
  .fiber-food-card {
    flex: 0 0 calc(30% - 20px);
    max-width: calc(30% - 20px);
  }
}

@media (max-width: 768px) {
  .fiber-food-card {
    flex: 0 0 calc(50% - 20px);
    max-width: calc(50% - 20px);
  }
}

@media (max-width: 480px) {
  .fiber-food-card {
    flex: 0 0 calc(50% - 10px);
    max-width: calc(50% - 10px);
  }
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
