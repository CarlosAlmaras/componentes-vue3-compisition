<script setup>
//todo lo que venga de vue o externa lo ponemos primero y despues componentes
import { ref, computed, onMounted } from 'vue';

//Componentes
import ButtonCounter from './components/ButtonCounter.vue';
import BlogPost from './components/BlogPost.vue'
import PaginatePost from './components/PaginatePost.vue'
import LoadingSpinner from './components/LoadingSpinner.vue'


const post = ref([]);
const postXpage = 10;//elemento que quiero mostrar en mi sitio web
const inicio = ref(0);
const fin = ref(postXpage);
const favorito = ref('');
const loading = ref(true);//inicializamos con true, para que apaarezca el spinner como primera instancia, ya que primero se carga el template por el Onmounted

const cambiarFAvorito = (post) => {
   favorito.value = post;
}
const next = ( ) => {
   inicio.value = inicio.value + postXpage;
   fin.value = fin.value + postXpage;
}
const prev = () => {
   inicio.value -= postXpage;
   fin.value -= postXpage;
}
onMounted(async() =>{
   // try {
   //    const rest = await fetch('https://jsonplaceholder.typicode.com/posts')//await solamente se puede utilizar para promesas
   //    post.value = await rest.json()
   // } catch (error) {
   //    console.log(error)
   // } finally {
   //    //loading.value = false
   //    setTimeout(() =>{
   //        loading.value = false
   //     }, 2000)
   // }

   //otra alternativa
// fetch('https://jsonplaceholder.typicode.com/posts')
//    .then(res => res.json())
//    .then(data => {
//       //console.log(data);
//       post.value = data;
//    })
//    .catch((e) => console.log(e))
//    .finally(() => {
//       loading.value = false
//       setTimeout(() =>{
//          loading.value = false
//       }, 2000)
//    }) 

//otra alternativa
//fetchData()
});// todo lo que tiene adentro, se ejecuta una vez que ya esta montado nuestro template

/*
Esto nos sirve cuando quieran acceder a un elemento que ya se haya montado, en su arbol DOM.
Tambien nos sirve para asegurarnos que el elemento se haya montado en el dom y nosotros acceder.
*/

//Altertaniva correcta, porque todavía no se carga el DOM y ya estamos haciendo la petición

// fetch('https://jsonplaceholder.typicode.com/posts')
//    .then(res => res.json())
//    .then(data => {
//       //console.log(data);
//       post.value = data;
//    })
//    .catch((e) => console.log(e))
//    .finally(() => {
//       //loading.value = false
//       setTimeout(() =>{
//          loading.value = false
//       }, 2000)//ejecuta lo que tiene dentro despues de 2 segundos, no lo recomiendo mucho el pprofesor porque forzamos a que se tarde, lo mejor es que se tarde lo menos posible en cargar la data, por temas de experiencia de usuario
//    }) //este se ejecuta independiente si .then funciona o no

/*EL fetch lo colocamos dentro del script y sin la necesidad de escribirlo dentro de otra función, porque?,
Porque el método setup se ejecuta antes de que nosotros tengamos montado nuestro componente. Otra ventaja 
del método setup, es que támbien podemos acceder a las variables reactivas.*/

//Otra alternativa con try catch y asynch // mas correcta

const fetchData = async() => {
   try {
      const rest = await fetch('https://jsonplaceholder.typicode.com/posts')
      post.value = await rest.json()
   } catch (error) {
      console.log(error)
   } finally {
      //loading.value = false
      setTimeout(() =>{
          loading.value = false
       }, 2000)
   }
};

fetchData();

const maxLength = computed(() => post.value.length);

/*
Vimos que en el setup podemos crear directamente el fetch, esto es valido porque el setup viene con un evento preestablecido que es el
created, este lo que hace es crear toda la lógica y luego monta todo nuestro sitio web.
En la api de opciones no viene por defecto y nosotoros podemos activarlo.

Hay otro ciclo de vida que nosotros podemos controlar.
El onmonted, una vez que nuestro template(html) esta montado, nosotros podemos activar un método en cuestión.
*/


</script>
<template>
   <LoadingSpinner v-if="loading"/>
   <div class="container" v-else>
    
      <h1>APP de Carlos</h1>
      <h2>Mi Post Favorito: {{ favorito }}</h2>

      <ButtonCounter/>
      <button-counter></button-counter>

      <!-- <button @click="next">Probando next</button>
      <button @click="prev">Probando previus</button> -->
      <h3>El total de post son: {{ maxLength }}</h3>
      <PaginatePost :inicio="inicio" :fin="fin" :max_post="maxLength" @prevEmit="prev" @nextEmit='next' class="m-3"/>

      <Blog-Post 
       v-for="p in post.slice(inicio, fin)" 
       :key="p.id"
       :title="p.title" 
       :id="p.id" 
       :body="p.body" 
       class="m-3"
       :cambiarFAvoritoProps = 'cambiarFAvorito'
       @cambiarFAvoritoNombre = 'cambiarFAvorito'
       ></Blog-Post>
       
      <!--Slice udemy-->
   </div>
  
</template>