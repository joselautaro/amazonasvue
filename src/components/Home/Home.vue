<template>
    <!-- Establecer un fondo oscuro y texto claro en toda la página -->
    <div class="bg-dark text-light" style="width: 100%;">
        <!-- Header -->
        <header class="bg-secondary">
            <!-- Contenedor principal del encabezado -->
            <div class="container py-3">
                <div class="row align-items-center">
                    <!-- Columna para el encabezado -->
                    <div class="col">
                        <h1 class="h3 mb-0">Amazonas</h1>
                    </div>
                    <!-- Columna con el input para buscar productos -->
                    <div class="col-auto">
                        <div class="input-group">
                            <!-- Campo de busqueda -->
                            <input type="text" v-model="searchTerm" class="form-control" placeholder="Buscar productos">
                            <button class="btn btn-outline-light" type="button">Buscar</button>
                        </div>
                    </div>
                    <!-- Columna para la vista del botón del carrito -->
                    <div class="col-auto">
                        <button @click="toggleCart" class="btn  position-relative">
                            
                            <el-icon color="#FFFFFF">
                                <ElementPlusIconsVue.ShoppingCart style="height: 25px;"/></el-icon>
                            <span
                                class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-danger">
                                {{ cartItemCount }}
                            </span>
                        </button>
                    </div>
                </div>
            </div>
        </header>


        <!-- _________________________________________________________________ -->

        <!-- Si no hay un producto seleccionado, se muestra la lista de productos -->
        <main class="container py-4" v-if="!selectedProduct">
            <div class="row justify-content-center">
                <!-- Ciclo para renderiza cada producto filtrado en una tarjeta -->
                <div v-for="product in filteredProduct" :key="product.id" class="col">
                    <!-- Cada tarjeta tendrá un ancho del 250px y el alto sera del 100% respecto de su contenedor -->
                    <div class="card h-100 bg-secondary text-light" style="width: 250px; margin-top: 25px;">
                        <!-- Cada imagen del producto, con ajuste para que siempre llene el espacio y mantenga la proporción -->
                        <img :src="product.image" alt="producto" class="card-img-top"
                            style="height: 250px; object-fit: cover;">
                        <div class="card-body">
                            <!-- Titulo del producto -->
                            <h5 class="card-title">{{ product.name }}</h5>
                            <!-- Descripción del producto -->
                            <p class="card-text"> {{ product.description }}</p>
                            <div class="d-flex justify-content-between align-item-center">
                                <!-- Precio del producto -->
                                <span class="h5 mb-0 text-white">{{ product.price.toFixed(2) }}</span>
                                <button @click="addToCart(product)" class="btn btn-primary">Añadir</button>
                            </div>
                            <button @click="viewProduct(product)" class="btn btn-outline-light mt-2">Ver
                                detalles</button>
                        </div>
                    </div>
                </div>
            </div>
        </main>


        <!-- Vista del detalle del producto -->
        <div v-else class="container py-4">
            <div class="row justify-content-center">
                <div class="col-md-8">
                    <!-- Detalle del producto seleccionado -->
                    <div class="card bg-secondary text-light">
                        <!--  -->
                        <img :src="selectedProduct.image" :alt="selectedProduct.name" class="card-img-top"
                            style="height: 400px; object-fit: cover;">
                        <div class="card-body">
                            <h3 class="card-title">{{ selectedProduct.name }}</h3>
                            <p class="card-text">{{ selectedProduct.description }}</p>
                            <h4 class="card-text">{{ selectedProduct.price }}</h4>
                            <button class="btn btn-primary" @click="addToCart(selectedProduct)">Añadir al
                                carrito</button>
                            <button class="btn btn-outline-light m-3" @click="selectedProduct = null">Regresar</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>


        <!-- Carrito de compras -->

        <div v-if="isCartOpen" class="position-fixed top-0 end-0 h-100 bg-dark p-3"
            style="width: 600px; z-index: 1050;">
            <div class="d-flex justify-content-between align-items-center mb-3">
                <h2 class="h4 mb-0 text-light">Carrito</h2>
                <button class="btn btn-close btn-close-white"> </button>
            </div>
            <!-- Muestra si el carrito está vacio -->
            <div class="text-center text-light">
                Carrito vacio
            </div>
            <!-- Mostrar los elementos del carrito si hay productos - poner v-else-->
            <div>
                <div class="card m-3 bg-secondary text-light">
                    <div class="card-body">
                        <div class="d-flex justify-content-between">
                            <!-- Nombre y precio al producto del carrito -->
                            <h5 class="card-title">Nombre</h5>
                            <p class="card-text">Precio: $</p>
                        </div>
                        <div class="d-flex align-items-center">
                            <!-- Botones para cambiar la cantidad de un producto -->
                            <button class="btn btn-sm btn-outline-light me-2">➖</button>
                            <span>0</span>
                            <button class="btn btn-sm btn-outline-light ms-2">➕</button>
                            <button class="btn btn-sm btn-danger ms-2">Eliminar</button>
                        </div>
                    </div>
                </div>
            </div>
            <div class="mt-3">
                <div class="d-flex justify-content-between align-items-center mb-3">
                    <!-- Total del carrito -->
                    <span class="h5 mb-0 text-light">Total: </span>
                    <span class="h5 mb-0 text-light">$150</span>
                </div>
                <button class="btn btn-primary w-100">Terminar compra</button>
            </div>
        </div>

        <!-- Notifición tipo toast -->
        <div class="position-fixed botton-0 end-0 p-3">
            <div class="toast show bg-success text-white" role="alert" aria-live="assertive" aria-atomic="true">
                <div class="toast-body">
                    Producto agregado satisfactoriamente
                </div>
            </div>
        </div>
    </div>
</template>


<script setup>
import * as ElementPlusIconsVue from '@element-plus/icons-vue'
import { ref, computed } from 'vue'

/* Definición de los productos */
const products = ref([
    {
        id: 1,
        name: "Wireless Headphones",
        description: "High-quality wireless headphones with noise cancellation",
        price: 129.99,
        image: "https://fastly.picsum.photos/id/758/200/300.jpg?hmac=lQtDVVjQGklGEIBCA-5yXBI3L8zkkeGObzmCi-rUFKo"
    },
    {
        id: 2,
        name: "Smartphone",
        description: "Latest model smartphone with advanced camera features",
        price: 699.99,
        image: "https://fastly.picsum.photos/id/758/200/300.jpg?hmac=lQtDVVjQGklGEIBCA-5yXBI3L8zkkeGObzmCi-rUFKo"
    },
    {
        id: 3,
        name: "Laptop",
        description: "Powerful laptop for work and entertainment",
        price: 1299.99,
        image: "https://fastly.picsum.photos/id/758/200/300.jpg?hmac=lQtDVVjQGklGEIBCA-5yXBI3L8zkkeGObzmCi-rUFKo"
    },
    {
        id: 4,
        name: "Smartwatch",
        description: "Track your fitness and receive notifications on the go",
        price: 199.99,
        image: "https://fastly.picsum.photos/id/758/200/300.jpg?hmac=lQtDVVjQGklGEIBCA-5yXBI3L8zkkeGObzmCi-rUFKo"
    }
])

//   Reactividad

const cart = ref([]) //Carrito de compras
const isCartOpen = ref(false) //Estado del carrito (abierto o cerrado)
const showToast = ref(false) //Estado de la toast ( notificacion)
const searchTerm = ref("") //Termino de busqueda
const selectedProduct = ref(null) //Producto seleccionado para ver en detalle

// Filtrado de productos según el termino de busqueda

const filteredProduct = computed(() => {
    return products.value.filter(product => product.name.toLowerCase().includes(searchTerm.value.toLowerCase()))
})

// Cantidad de productos que hay en el carrito
const cartItemCount = computed(() => cart.value.reduce((count, item) => count + item.quantity, 0))

// Funcion para añadir productos al carrito
const addToCart = (product) => {
    const existingItem = cart.value.find(item => item.id === product.id)
    if (existingItem) {
        existingItem.quantity++
    } else {
        cart.value.push({ ...product, quantity: 1 })
    }
    // Agregamos el valor de showToast
    showToast.value = true
    setTimeout(() => {
        showToast.value = false
    }, 3000);
}

// Funcion para poder abrir y cerrar el carrito
const toggleCart = () => {
    isCartOpen.value = !isCartOpen.value
}

// Funcion para poder actualizar la cantidad de producto en el carrito
const updateQuantity = (productId, newQuantity) => {
    const item = cart.value.find(item => item.id === productId)
    if (item && newQuantity > 0) {
        item.quantity = newQuantity
    } else {
        removeFromCart(productId)
    }
}

// Funcion para eliminar un producto del carrito
const removeFromCart = (productId) => {
    cart.value = cart.value.filter(item => item.id !== productId)
}

// Funcion para ver los detalles del producto
const viewProduct = (product) => {
    selectedProduct.value = product
}

</script>