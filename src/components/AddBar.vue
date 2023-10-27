<script setup>
import {defineProps, defineEmits} from 'vue'
const props = defineProps({
  modelValue: String,
  create: Function
})

defineEmits(['update:modelValue'])

</script>

<template>
    <div class="flex items-center justify-center gap-4">
        <input type="text" placeholder="Qual é a tarefa?" class="w-full p-4 text-white rounded outline-none bg-slate-800" :value="modelValue" @input="$emit('update:modelValue', $event.target.value)">
        <button class="p-4 text-white transition-all bg-green-600 rounded hover:bg-green-700" @click="create">Adicionar</button>
    </div>
</template>