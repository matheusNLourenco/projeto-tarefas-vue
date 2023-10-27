<script setup>
    import { defineProps, defineEmits } from 'vue'
    
    const props = defineProps({
        filter: String,
        countTasks: Function
    })
    
    defineEmits(['click:filter'])

</script>

<template>
    <div class="flex items-center justify-center gap-4 my-6">
        <button 
            :class="props.filter === 'pendente' ? 'border-green-600' : 'border-b-transparent'" 
            class="relative w-full p-2 text-white border-0 border-b-2" 
            @click="$emit('click:filter', props.filter = 'pendente')"
        >
            Pendentes
            <span v-show="props.countTasks('pendente') > 0" class="absolute right-0 -top-3 text-white bg-red-700 rounded-[100%] w-[20px] h-[20px] text-sm flex justify-center items-center">{{ props.countTasks('pendente') }}</span>
        </button>
        
        <button 
            :class="props.filter === 'concluido' ? 'border-green-600' : 'border-b-transparent'" 
            class="relative w-full p-2 text-white border-0 border-b-2" 
            @click="$emit('click:filter', props.filter = 'concluido')"
        >
            Concluidas
            <span v-show="props.countTasks('concluido') > 0" class="absolute right-0 -top-3 text-white bg-red-700 rounded-[100%] w-[20px] h-[20px] text-sm flex justify-center items-center">{{ props.countTasks('concluido') }}</span>
        </button>
    </div>
</template>