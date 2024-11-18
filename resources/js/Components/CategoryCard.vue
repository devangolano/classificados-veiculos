<template>
    <div class="min-h-screen">
        <!-- Main Content -->
        <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
            <!-- Hero Section -->
            <div class="text-center text-2xl md:text-4xl lg:text-6xl mb-10">
                <h1 class="font-bold text-blue-950">
                    Você está procurando um carro?
                </h1>
            </div>

            <!-- Search Bar -->
            <div class="max-w-2xl mx-auto mb-8 md:mb-12">
                <div class="relative">
                    <input
                        type="text"
                        placeholder="Procurar imóveis ou veículos"
                        class="w-full px-4 py-3 pl-16 border border-blue-950 rounded-full focus:outline-none focus:ring-2 focus:ring-r-blue-950 focus:border-transparent"
                    />
                    <div
                        class="absolute left-4 top-1/2 transform -translate-y-1/2"
                    >
                        <SearchIcon
                            class="h-9 w-9 bg-blue-600 p-2 rounded-full text-white"
                        />
                    </div>
                </div>
            </div>

            <section class="mb-12">
                <h2 class="text-xl font-semibold text-blue-950 mb-6">
                    Confira as marcas mais procuradas
                </h2>

                <div
                    ref="containerRef"
                    class="flex gap-4 items-center mb-8 pb-4 scrollbar-hide"
                    style="width: 100%; overflow-x: hidden"
                >
                    <div
                        :style="{
                            transform: `translateX(${-scrollPosition}px)`,
                            transition: 'transform 25s linear',
                        }"
                        class="flex gap-4 items-center"
                    >
                        <button
                            v-for="brand in duplicatedBrands"
                            :key="brand.id"
                            @click="selectBrand(brand.name)"
                            class="flex flex-col items-center min-w-[155px] p-4 border rounded-lg hover:border-blue-600 transition-colors duration-200"
                            :class="{
                                'border-blue-600': activeBrand === brand.name,
                                'border-gray-200': activeBrand !== brand.name,
                            }"
                        >
                            <img
                                :src="brand.logo"
                                :alt="brand.name"
                                class="h-6 mb-2"
                            />
                            <span class="text-sm text-gray-900">{{
                                brand.name
                            }}</span>
                        </button>
                    </div>
                </div>
            </section>

            <ListCar />
            <!-- Most Searched Tags -->
            <section class="mb-12">
                <h2 class="text-xl font-semibold text-blue-950 mb-4">
                    Mais buscados
                </h2>
                <div class="flex flex-wrap gap-2">
                    <button
                        v-for="tag in searchTags"
                        :key="tag"
                        class="px-4 py-2 bg-gray-100 hover:bg-gray-200 rounded-full text-sm text-gray-900 transition-colors duration-200"
                    >
                        {{ tag }}
                    </button>
                </div>
            </section>
        </main>
    </div>
</template>

<script setup>
import ListCar from "@/Components/ListCar.vue";
import {
    SearchIcon,
    HomeIcon,
    CarIcon,
    BuildingIcon,
    Building2,
    Car,
    User,
    ArrowRight,
    Megaphone,
} from "lucide-vue-next";

import { ref, computed, onMounted, onUnmounted } from "vue";

const containerRef = ref(null);
const scrollPosition = ref(0);
let animationId = null;

const activeBrand = ref("Chevrolet");

function selectBrand(brand) {
    activeBrand.value = brand;
}

const filteredCarModels = computed(() =>
    carModels.filter((model) => model.brand === activeBrand.value)
);

const duplicatedBrands = computed(() => {
    return [...carBrands, ...carBrands].map((brand, index) => ({
        ...brand,
        id: `${brand.name}-${index}`,
    }));
});

const startAutoScroll = () => {
    if (containerRef.value) {
        const animate = () => {
            scrollPosition.value += 1;
            if (scrollPosition.value >= containerRef.value.scrollWidth / 2) {
                scrollPosition.value = 0;
            }
            animationId = requestAnimationFrame(animate);
        };
        animate();
    }
};

onMounted(() => {
    startAutoScroll();
});

onUnmounted(() => {
    if (animationId) {
        cancelAnimationFrame(animationId);
    }
});
const searchCards = [
    {
        title: "Quero comprar um carro",
        image: "https://business.citroen.pt/content/dam/citroen/portugal/b2b/models/adapted-cars/TAXI_eC4X.png?imwidth=768",
    },
    {
        title: "Estou procurando um apartamento para comprar",
        image: "https://www.verangola.net/va/images/cms-image-000021820.png",
    },
    {
        title: "Estou procurando um imóvel para alugar",
        image: "https://res.listglobally.com/listings/6193731/100918097/f2a58329166827289ed3cb2033fea723?mode=crop&height=300",
    },
];

const searchTags = [
    "Carro Novo à venda",
    "Kia Rio 2020",
    "Ford Ranger",
    "Carros Semi Novo",
];

// Lista de marcas de carros
const carBrands = [
    {
        name: "Chevrolet",
        logo: "https://www.chavesnamao.com.br/cdn-cgi/image/width=50,height=28,format=auto,fit=contain,quality=75/https://cdn.chavesnamao.com.br/portal/vehicle/logo/carro/chevrolet.png",
    },
    {
        name: "Hyundai",
        logo: "https://www.chavesnamao.com.br/cdn-cgi/image/width=50,height=28,format=auto,fit=contain,quality=75/https://cdn.chavesnamao.com.br/portal/vehicle/logo/carro/hyundai.png",
    },
    {
        name: "Fiat",
        logo: "https://www.chavesnamao.com.br/cdn-cgi/image/width=50,height=28,format=auto,fit=contain,quality=75/https://cdn.chavesnamao.com.br/portal/vehicle/logo/carro/fiat.png",
    },
    {
        name: "Renault",
        logo: "https://www.chavesnamao.com.br/cdn-cgi/image/width=50,height=28,format=auto,fit=contain,quality=75/https://cdn.chavesnamao.com.br/portal/vehicle/logo/carro/renault.png",
    },
    {
        name: "Ford",
        logo: "https://www.chavesnamao.com.br/cdn-cgi/image/width=50,height=28,format=auto,fit=contain,quality=75/https://cdn.chavesnamao.com.br/portal/vehicle/logo/carro/ford.png",
    },
    {
        name: "Honda",
        logo: "https://www.chavesnamao.com.br/cdn-cgi/image/width=50,height=28,format=auto,fit=contain,quality=75/https://cdn.chavesnamao.com.br/portal/vehicle/logo/carro/honda.png",
    },

    {
        name: "Volkswagen",
        logo: "https://www.chavesnamao.com.br/cdn-cgi/image/width=50,height=28,format=auto,fit=contain,quality=75/https://cdn.chavesnamao.com.br/portal/vehicle/logo/carro/volkswagen.png",
    },

    {
        name: "Toyota",
        logo: "https://www.chavesnamao.com.br/cdn-cgi/image/width=50,height=28,format=auto,fit=contain,quality=75/https://cdn.chavesnamao.com.br/portal/vehicle/logo/carro/toyota.png",
    },
];

const categoriesCity = [
    {
        icon: CarIcon,
        title: "Carros à venda",
        subtitle: "Seu próximo veículo",
    },
    {
        icon: BuildingIcon,
        title: "Novidades para você",
        subtitle: "Confira as ofertas",
    },
];
</script>

<style scoped>
.scrollbar-hide::-webkit-scrollbar {
    display: none;
}
.scrollbar-hide {
    -ms-overflow-style: none;
    scrollbar-width: none;
}
</style>
