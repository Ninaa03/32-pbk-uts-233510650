<template>
  <main>
    <h1>Daftar Aktivitas Harian</h1>

    <div class="input-group">
      <input
        v-model="newTodo"
        placeholder="aktivitas baru..."
        @keyup.enter="addTodo"
      />
      <button @click="addTodo">Tambah</button>
    </div>

    <label class="filter">
      <input type="checkbox" v-model="showUncompletedOnly" />
      Tampilkan hanya kegiatan yang belum selesai
    </label>

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
const showUncompletedOnly = ref(false)

const addTodo = () => {
  if (newTodo.value.trim()) {
    todos.value.push({ text: newTodo.value.trim(), completed: false })
    newTodo.value = ''
  }
}

const removeTodo = (index) => {
  todos.value.splice(index, 1)
}

const filteredTodos = computed(() =>
  showUncompletedOnly.value ? todos.value.filter(t => !t.completed) : todos.value
)
</script>

