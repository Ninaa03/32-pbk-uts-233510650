<template>
  <main>
    <h1>Daftar Tugas Harian</h1>

    <div class="input-group">
      <input
        v-model="newTodo"
        placeholder="aktivitas baru..."
        @keyup.enter="addTodo"
      />
      <button @click="addTodo">Tambah</button>
    </div>

    <div class="filter-group">
      <button @click="toggleFilterMenu">
        Filter: {{ filterLabel }}
      </button>
      <div v-if="showFilterMenu" class="filter-options">
        <button @click="setFilter('all')">Tampilkan Semua</button>
        <button @click="setFilter('uncompleted')">Tampilkan yang belum selesai</button>
        <button @click="setFilter('completed')">Tampilkan yang sudah selesai</button>
      </div>
    </div>

    <ul v-if="filteredTodos.length > 0" class="todo-list">
      <li
        v-for="(todo, index) in filteredTodos"
        :key="index"
        :class="{ completed: todo.completed }"
      >
        <input type="checkbox" v-model="todo.completed" />
        <span>{{ todo.text }}</span>
        <button @click="removeTodo(index)">Hapus</button>
      </li>
    </ul>
    <p v-else class="empty">Belum ada kegiatan.</p>
  </main>
</template>

<script setup>
import { ref, computed } from 'vue'

const newTodo = ref('')
const todos = ref([])
const filter = ref('all') 
const showFilterMenu = ref(false)

const toggleFilterMenu = () => {
  showFilterMenu.value = !showFilterMenu.value
}

const setFilter = (value) => {
  filter.value = value
  showFilterMenu.value = false
}

const addTodo = () => {
  if (newTodo.value.trim()) {
    todos.value.push({ text: newTodo.value.trim(), completed: false })
    newTodo.value = ''
  }
}

const removeTodo = (index) => {
  todos.value.splice(index, 1)
}

const filteredTodos = computed(() => {
  if (filter.value === 'completed') return todos.value.filter(t => t.completed)
  if (filter.value === 'uncompleted') return todos.value.filter(t => !t.completed)
  return todos.value
})

const filterLabel = computed(() => {
  if (filter.value === 'completed') return 'Sudah Selesai'
  if (filter.value === 'uncompleted') return 'Belum Selesai'
  return 'Semua'
})
</script>