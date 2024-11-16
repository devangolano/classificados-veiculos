<script setup>
import { Head } from "@inertiajs/vue3";
import AppLayout from "@/Layouts/AppLayout.vue";
import CategoryCard from "@/Components/CategoryCard.vue";

function handleImageError() {
    document.getElementById("screenshot-container")?.classList.add("!hidden");
}
</script>

<template>
    <Head title="Classificados de Veículos e Imóveis" />
    <AppLayout title="Classificados de Veículos e Imóveis">
        <div class="mt-6">
            <CategoryCard />
        </div>
    </AppLayout>
</template>
