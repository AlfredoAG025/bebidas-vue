<script setup>
import { computed } from 'vue'
import { RouterLink, useRoute } from 'vue-router'
import { useBebidasStore } from '../stores/bebidas'
import { useNotificacionStore } from '../stores/notificaciones'

const route = useRoute()
const store = useBebidasStore()
const notificaciones = useNotificacionStore()

const paginaInicio = computed(() => route.name === 'inicio')

const handleSubmit = () => {
    if (Object.values(store.busqueda).includes('')) {
        notificaciones.$patch({
            texto: 'Todos los campos son obligatorios',
            mostrar: true,
            error: true,
        })
        return
    }
    store.obtenerRecetas()
}

</script>

<template>
    <header class="bg-slate-800" :class="{ header: paginaInicio }">
        <div class="mx-auto container px-5 py-16">
            <div class="flex justify-between items-center">
                <div>
                    <RouterLink :to="{ name: 'inicio' }">
                        <img class="w-32" src="/img/logo.svg" alt="Logotipo">
                    </RouterLink>
                </div>

                <nav class="flex gap-4">
                    <RouterLink :to="{ name: 'inicio' }" class="text-white uppercase font-bold"
                        active-class="text-orange-500">
                        Inicio
                    </RouterLink>

                    <RouterLink :to="{ name: 'favoritos' }" class="text-white uppercase font-bold"
                        active-class="text-orange-500">
                        Favoritos
                    </RouterLink>
                </nav>
            </div>

            <form @submit.prevent="handleSubmit" v-if="paginaInicio"
                class="md:w-1/2 2xl:w-1/3 bg-orange-400 my-32 p-10 rounded-lg shadow space-y-6">
                <div class="space-y-4">
                    <label class="block text-white uppercase font-extrabold text-lg" for="ingrediente">Nombre o
                        Ingredientes</label>
                    <input v-model="store.busqueda.nombre" id="ingrediente"
                        class="p-3 w-full rounded-lg focus:outline-none"
                        placeholder="Nombre o Ingrediente ej. Vodka, Tequila, etc." type="text">
                </div>

                <div class="space-y-4">
                    <label class="block text-white uppercase font-extrabold text-lg" for="categoria">Categoría</label>
                    <select v-model="store.busqueda.categoria" id="categoria"
                        class="p-3 w-full rounded-lg focus:outline-none">
                        <option value="">-- Seleccione --</option>
                        <option :value="categoria.strCategory" :key="categoria.strCategory"
                            v-for="categoria in store.categorias">{{ categoria.strCategory }}</option>
                    </select>
                </div>

                <input type="submit"
                    class="bg-orange-800 hover:bg-orange-900 cursor-pointer text-white font-extrabold w-full p-2 rounded-lg uppercase"
                    value="Buscar Recetas" />
            </form>
        </div>
    </header>
</template>

<style scoped>
.header {
    background-image: url('/img/bg.jpg');
    background-size: cover;
    background-position: center
}
</style>