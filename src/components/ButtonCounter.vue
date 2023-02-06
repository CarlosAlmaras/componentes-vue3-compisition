<script setup>
import { ref } from 'vue';


const counter = ref(0);
const aumentar = () => counter.value++;
</script>

<template>
    <button @click="aumentar">{{counter}}</button>
</template>