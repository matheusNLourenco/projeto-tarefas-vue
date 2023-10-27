<script setup>
  import {ref, reactive} from 'vue'
  import AddBar from './components/AddBar.vue';
  import Tabs from './components/Tabs.vue';
  import TodoList from './components/TodoList.vue';

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
  <div class="flex items-center justify-center w-full min-h-screen bg-slate-950">
    <div class="container flex items-center justify-center px-4 mx-auto">
      <div class="px-5 py-10 w-96 min-h-[500px] bg-slate-900 rounded">
          <AddBar 
            :create="create"
            v-model="todoText"
          />
          
          <Tabs 
            :count-tasks="countTasks"
            :filter="filter"
            @click:filter="filter = $event"
          />

          <TodoList 
            :filter-category="() => filterCategory(filter)"
            :filter="filter"
            :update="update"
            :destroy="destroy"
          />
      </div>
    </div>
  </div>
</template>
