<script setup>
  import {ref, reactive} from 'vue'
  
  const todos = reactive([])
  const filter = ref('pendente')
  const id = ref(1)
  const todoText = ref('')

  function create() {
    todos.push({
      id: id.value++,
      todo: todoText.value,
      completed: false
    })

    todoText.value = ''
  }

  function update(todo) {
    const index = todos.findIndex(t => t.id === todo.id)

    todos.splice(index, 1, {...todo, completed: !todo.completed})
  }

  function destroy(todo) {
    const index = todos.findIndex(t => t.id === todo.id)
    todos.splice(index, 1)
  }

  function filterCategory(category) {
    if(category === 'pendente'){
      return todos.filter(todo => !todo.completed)
    }else if(category === 'concluido'){
      return todos.filter(todo => todo.completed)
    }

    return todos
  }

  function countTasks(status) {
    if(status === 'pendente') {
      return todos.filter(todo => !todo.completed).length
    }else {
      return todos.filter(todo => todo.completed).length
    }
  }

</script>

<template>
  <div class="w-full min-h-screen bg-slate-950 flex justify-center items-center">
    <div class="container mx-auto px-4 flex justify-center items-center">
      <div class="px-5 py-10 w-96 min-h-[500px] bg-slate-900 rounded">
          <div class="flex justify-center items-center gap-4">
            <input type="text" placeholder="Qual é a tarefa?" class="w-full bg-slate-800 rounded outline-none p-4 text-white" v-model="todoText">
            <button class="p-4 bg-green-600 rounded hover:bg-green-700 text-white transition-all" @click="create">Adicionar</button>
          </div>
          <div class="flex justify-center items-center my-6 gap-4">
            <button :class="filter === 'pendente' ? 'border-green-600' : 'border-b-transparent'" class="relative w-full p-2 border-0 border-b-2 text-white" @click="filter = 'pendente'">
              Pendentes
              <span v-show="countTasks('pendente') > 0" class="absolute right-0 -top-3 text-white bg-red-700 rounded-[100%] w-[20px] h-[20px] text-sm flex justify-center items-center">{{ countTasks('pendente') }}</span>
            </button>
            <button :class="filter === 'concluido' ? 'border-green-600' : 'border-b-transparent'" class="relative w-full p-2 border-0 border-b-2 text-white" @click="filter = 'concluido'">
              Concluidas
              <span v-show="countTasks('concluido') > 0" class="absolute right-0 -top-3 text-white bg-red-700 rounded-[100%] w-[20px] h-[20px] text-sm flex justify-center items-center">{{ countTasks('concluido') }}</span>
            </button>
          </div>
          <ul class="space-y-4">
            <li v-for="todo in filterCategory(filter)" :key="todo.id" class="relative flex justify-between items-center gap-4 bg-slate-800 p-2 rounded">
              <span class="break-words text-white w-64">{{ todo.todo }}</span>
              <span><input type="checkbox" :checked="todo.completed" @change="update(todo)"/></span>
              <span class="absolute -right-1 -top-3 text-red-700 cursor-pointer" @click="destroy(todo)">X</span>
            </li>
            <li v-show="filterCategory('pendente').length <= 0 && filter !== 'concluido'" class="text-white text-center">Nenhuma tarefa para ser feita.</li> 
          </ul>
      </div>
    </div>
  </div>
</template>
