<script setup>
import { Head } from "@inertiajs/vue3";
import CategoryCard from "@/Components/CategoryCard.vue";
import Header from "@/Components/Header.vue";

function handleImageError() {
    document.getElementById("screenshot-container")?.classList.add("!hidden");
}
</script>

<template>
    <Head title="Classificados de Veículos e Imóveis" />
    <Header />
    <CategoryCard />
</template>
