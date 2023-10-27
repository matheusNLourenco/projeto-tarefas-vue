<script setup>
    import {defineProps} from 'vue'

    const props = defineProps({
        filterCategory: Function,
        filter: String,
        update: Function,
        destroy: Function
    })
</script>

<template>
    <ul class="space-y-4">
        <li v-for="todo in props.filterCategory(props.filter)" :key="todo.id" class="relative flex items-center justify-between gap-4 p-2 rounded bg-slate-800">
            <span class="w-64 text-white break-words">{{ todo.todo }}</span>
            <span><input type="checkbox" :checked="todo.completed" @change="props.update(todo)"/></span>
            <span class="absolute text-red-700 cursor-pointer -right-1 -top-3" @click="props.destroy(todo)">X</span>
        </li>
        <li v-show="props.filterCategory('pendente').length <= 0 && props.filter !== 'concluido'" class="text-center text-white">Nenhuma tarefa para ser feita.</li> 
    </ul>
</template>