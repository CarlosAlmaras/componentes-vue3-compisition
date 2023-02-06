<script setup>
//Multiples props
//const props = defineProps(['title', 'id', 'body', 'colorText']); Tambien podría estar aqui la función cambiarFAvoritoProps

const props = defineProps({
    title: String,
    id: Number,
    body: {
        type: String,
        default: 'Sin descripción',
    },
    cambiarFAvoritoProps: Function
});

// console.log(props);
// console.log(props.title);

 const emit = defineEmits(['cambiarFAvoritoNombre']);

</script>

<template>
    <div class="card">
        <div class="card-body">
            <h5 class="card-title">{{ id }}.- {{ title }}</h5>

            <p>{{ body }}</p>
           
            <button 
             class="btn btn-outline-primary" 
             @click="cambiarFAvoritoProps(title)"
             >Add Favorito con Props</button>
             <button 
             class="btn btn-outline-success m-2" 
             @click="emit('cambiarFAvoritoNombre', title)"
             >Add Favorito con Emit</button>

        </div>
    </div>
</template>