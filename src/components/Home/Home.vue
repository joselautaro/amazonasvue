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
                        <button class="btn btn-primary position-relative">
                            Carrito
                            <span
                                class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-danger">
                                0
                            </span>
                        </button>
                    </div>
                </div>
            </div>
        </header>
    </div>

</template>