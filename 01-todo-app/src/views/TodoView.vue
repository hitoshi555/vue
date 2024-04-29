<script setup lang="ts">
import { ref, computed } from 'vue'

type ToDo = {
  id: number
  text: string
  done: boolean
}

let id = 0

const newTodo = ref('')
const hideCompleted = ref(false)
const todos = ref<ToDo[]>([
  { id: id++, text: 'Learn HTML', done: true },
  { id: id++, text: 'Learn JavaScript', done: true },
  { id: id++, text: 'Learn Vue', done: false }
])

const filteredTodos = computed(() => {
  return hideCompleted.value ? todos.value.filter((t) => !t.done) : todos.value
})

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value, done: false })
  newTodo.value = ''
}

function removeTodo(todo: ToDo) {
  todos.value = todos.value.filter((t) => t !== todo)
}

function setDone(todo: ToDo) {
  todo.done = !todo.done
}
</script>

<template>
  <div class="h-100 w-full flex items-center justify-center bg-teal-lightest font-sans">
    <div class="bg-white rounded shadow p-6 m-4 w-full lg:w-3/4 lg:max-w-lg">
      <div class="mb-4">
        <h1 class="text-grey-darkest">Todo List</h1>
        <form @submit.prevent="addTodo">
          <div class="flex mt-4">
            <input
              v-model="newTodo"
              required
              class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-grey-darker"
              placeholder="Add Todo"
            />
            <button
              class="flex-no-shrink p-2 border-2 rounded text-teal border-teal hover:text-gray-400 hover:bg-slate-100"
            >
              Add
            </button>
          </div>
        </form>
      </div>
      <div>
        <div v-for="todo in filteredTodos" :key="todo.id" class="flex mb-4 items-center">
          <p class="w-full text-grey-darkest" :class="{ done: todo.done }">{{ todo.text }}</p>
          <button
            @click="setDone(todo)"
            class="flex-no-shrink p-2 ml-4 mr-2 border-2 rounded hover:text-gray-400 hover:bg-slate-100"
          >
            Done
          </button>
          <button
            @click="removeTodo(todo)"
            class="flex-no-shrink p-2 ml-2 border-2 rounded hover:text-gray-400 hover:bg-slate-100"
          >
            Remove
          </button>
        </div>
      </div>
      <button @click="hideCompleted = !hideCompleted" class="flex-no-shrink p-2 border-2 rounded hover:text-gray-400 hover:bg-slate-100"
        >
        {{ hideCompleted ? 'Show all' : 'Hide completed' }}
      </button>
    </div>
  </div>
</template>

<style>
.done {
  text-decoration: line-through;
}
</style>
