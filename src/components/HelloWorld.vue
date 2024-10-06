<script setup>
import { onMounted, ref } from "vue";

defineProps({
  msg: String,
  greet: String,
});

const count = ref(0);
const status = ref(true);
const tasks = ref(["eat", "play", "sleep", "code", "concert"]);

const toggleStatus = () => {
  console.log("Getting invoked", status);
  status.value = !status.value;
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
};

onMounted(async () => {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/todos");
    const data = await response.json();
    tasks.value = data.map((task) => task.title);
  } catch (error) {
    console.log(error);
  }
});
</script>

<template>
  <h1>{{ msg }}</h1>
  <h2>{{ greet }}</h2>
  <div class="card">
    <button type="button" @click="count++">count is {{ count }}</button>
    <button @click="toggleStatus">Change Status</button>
    <ul>
      <li v-for="(task, index) in tasks" :key="task">
        <span>
          {{ task }}
        </span>
        <button @click="deleteTask(index)">X</button>
      </li>
    </ul>
  </div>

  <p v-if="status">Active</p>
  <p v-else>InActive</p>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
