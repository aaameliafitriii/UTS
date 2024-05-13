<template>
  <div class="container">
    <header>
      <nav>
        <ul>
          <li @click="showTodos">Todos</li>
          <li @click="showPosts">Posts</li>
        </ul>
      </nav>
    </header>
    <div class="todo-app" v-if="activeTab === 'todos'">

      <h1>Kegiatan-Ku! <br> Hal yang harus dilakukan</h1>
      <div class="row">
        <input type="text" v-model="newTask" placeholder="Ketik kegiatan">
        <button @click="addTask">ADD</button>
      </div>

      <div class="filters">
        <button @click="hideCompleted = !hideCompleted" class="button-74">
          {{ hideCompleted ? 'sembunyikan' : 'tampilkan' }} yang selesai
        </button>
      </div>

      <ul>
        <li v-for="(task, index) in filteredTodos" :key="task.id" :class="{ checked: task.checked }" @click="toggleTask(task)">
          {{ task.text }}
          <span @click.stop="removeTask(task)">&#xd7;</span>
        </li>
      </ul>
    </div>

    <div class="post-app" v-if="activeTab === 'posts'">
      <!-- Tambahkan konten postingan pengguna di sini -->
    </div>

  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const newTask = ref('');
const tasks = ref([]);
const hideCompleted = ref(false);
const muted = ref(false);
const volume = ref(1);
const activeTab = ref('todos'); // Menyimpan tab yang sedang aktif

const filteredTodos = computed(() => {
  return hideCompleted.value ? tasks.value.filter(task => !task.checked) : tasks.value;
});

function addTask() {
  if (newTask.value.trim() === '') {
    alert("Isi terlebih dahulu!");
  } else {
    tasks.value.unshift({ id: Date.now(), text: newTask.value, checked: false });
    newTask.value = '';
    saveData();
  }
}

function toggleTask(task) {
  task.checked = !task.checked;
  saveData();
}

function removeTask(task) {
  const index = tasks.value.findIndex(t => t.id === task.id);
  if (index !== -1) {
    tasks.value.splice(index, 1);
    saveData();
  }
}

function saveData() {
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
}

function loadData() {
  const savedTasks = localStorage.getItem("tasks");
  tasks.value = savedTasks ? JSON.parse(savedTasks) : [];
}

function showTodos() {
  activeTab.value = 'todos';
}

function showPosts() {
  activeTab.value = 'posts';
}

loadData();
</script>
