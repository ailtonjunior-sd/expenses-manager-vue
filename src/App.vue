<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
    <header>
        <div class="h-16 bg-white filter drop-shadow">
            <nav class="flex items-center h-full space-x-5 w-fit mx-auto">
                <RouterLink to="/" class="h-full py-5 px-4 text-gray-400 font-medium hover:bg-gray-200 hover:text-gray-600 transition-all ease-in-out duration-150">Início</RouterLink>
                <RouterLink to="/entries" class="h-full py-5 px-4 text-gray-400 font-medium hover:bg-gray-200 hover:text-gray-600 transition-all ease-in-out duration-150">Lançamentos</RouterLink>
                <RouterLink to="/about" class="h-full py-5 px-4 text-gray-400 font-medium hover:bg-gray-200 hover:text-gray-600 transition-all ease-in-out duration-150">Sobre</RouterLink>
            </nav>
        </div>
    </header>

    <RouterView class="container mx-auto py-4" />
</template>

<style>
html, body, #app {
    height: 100%;
    background-color: rgb(230, 230, 230);
}
</style>

