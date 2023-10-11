<script setup>
import { ref, reactive, onMounted } from 'vue'
import { db } from './data/guitarras'
import Guitarra from './components/guitarra.vue'
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'

// const state = reactive({    //si son objetos datos relacionados utilizar reactive
//     guitarras: []
// });
// console.log(state.guitarras);

const guitarras = ref([]);    //si son otros tipos de datos ejem bool string etc utilizar ref
const carrito = ref([])
const guitarra = ref({})

onMounted(() => {//metodo que se ejecuta cuando el componente esta montado
    guitarras.value = db;//aqui podria hacer un get a una api con fetch para traer los datos
    //state.guitarras = db;

    guitarra.value = db[ 3 ]
});

const agregarCarrito = (guitarra) => {
    const existeCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id);

    if (existeCarrito >= 0) {
        carrito.value[ existeCarrito ].cantidad++
    } else {
        guitarra.cantidad = 1;
        carrito.value.push(guitarra);
    }
}

const decrementarCantidad = (id) => {
    const index = carrito.value.findIndex(producto => producto.id === id);

    if (carrito.value[ index ].cantidad <= 1) {
        return;
    }
    else {
        carrito.value[ index ].cantidad--;
    }
}

const incrementarCantidad = (id) => {
    const index = carrito.value.findIndex(producto => producto.id === id);

    if (carrito.value[ index ].cantidad >= 10) {
        return;
    }
    else {
        carrito.value[ index ].cantidad++;
    }
}

const eliminarProducto = (id) => {
    carrito.value = carrito.value.filter(producto => producto.id !== id)
}

const vaciarCarrito = () => {
    carrito.value = []
}

</script>

<template>
    <Header :carrito="carrito" :guitarra="guitarra" @incrementar-cantidad="incrementarCantidad"
        @decrementar-cantidad="decrementarCantidad" @agregar-carrito="agregarCarrito" @eliminar-producto="eliminarProducto"
        @vaciar-carrito="vaciarCarrito" />
    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
            <Guitarra v-for="guitarra in guitarras" v-bind:guitarra="guitarra" @agregar-carrito="agregarCarrito" />
        </div>
    </main>
    <Footer />
</template>