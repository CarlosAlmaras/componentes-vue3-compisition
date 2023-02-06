<script setup>
const emit = defineEmits(['nextEmit']);

const activarSiguiente = () =>{
    console.log('Me diste click');
    emit('nextEmit')
}
const props = defineProps(['inicio', 'fin', 'max_post']);

</script>

<template>
    <div class="btn-group" role="group" aria-label="Basic example">
        <button @click="$emit('prevEmit')"
         type="button" 
         class="btn btn-outline-primary"
         :disabled="inicio <= 0"
        >
         Previus {{ inicio }}
        </button>

        <button 
         @click="activarSiguiente"
         type="button" 
         class="btn btn-outline-danger"
         :disabled="fin >= max_post"
         >
         Next {{ fin }}
        </button>
    </div>
</template>