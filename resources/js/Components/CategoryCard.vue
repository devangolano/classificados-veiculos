<template>
    <div class="min-h-screen">
        <!-- Main Content -->
        <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
            <!-- Hero Section -->
            <div class="text-center text-2xl md:text-4xl lg:text-6xl mb-10">
                <h1 class="font-bold text-blue-950">
                    Carros, Imóveis e Motos.
                </h1>
                <p class="text-blue-600 font-semibold">Oque você procura!</p>
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

            <!-- Category Cards -->
            <div
                class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4 mb-14"
            >
                <div
                    v-for="category in categoriesCity"
                    :key="category.title"
                    class="bg-white p-4 rounded-lg shadow-md hover:shadow-xl transition-shadow duration-200 cursor-pointer"
                >
                    <div class="flex flex-col items-center">
                        <component
                            :is="category.icon"
                            class="h-8 w-8 text-blue-600 mb-2"
                        />
                        <h3 class="text-md font-medium text-gray-900">
                            {{ category.title }}
                        </h3>
                        <p class="text-sm text-gray-500">
                            {{ category.subtitle }}
                        </p>
                    </div>
                </div>
            </div>

            <!-- City Sections -->
            <h2 class="text-xl font-semibold text-blue-950 mb-5">
                Apartamentos e Casas
            </h2>
            <div
                class="grid grid-cols-1 md:grid-cols-2 mb-12 lg:grid-cols-4 gap-6"
            >
                <div
                    v-for="city in cities"
                    :key="city.name"
                    class="group cursor-pointer"
                >
                    <div class="relative overflow-hidden rounded-lg">
                        <img
                            :src="city.image"
                            :alt="city.name"
                            class="w-full h-48 object-cover"
                        />
                        <div
                            class="absolute inset-0 bg-gradient-to-t from-black/60 to-transparent"
                        >
                            <div class="absolute bottom-4 left-4">
                                <h3 class="text-white font-semibold mb-1">
                                    Imóveis em {{ city.name }}
                                </h3>
                                <div class="space-y-1">
                                    <a
                                        v-for="link in city.links"
                                        :key="link"
                                        class="block text-sm text-white/90 hover:text-white"
                                    >
                                        {{ link }}
                                    </a>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

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

            <section class="mb-12">
                <h2 class="text-xl font-semibold text-blue-950 mb-6">
                    Confira os carros mais procurados
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

                <div
                    class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-5 gap-6"
                >
                    <div
                        v-for="model in filteredCarModels"
                        :key="model.name"
                        class="group cursor-pointer hover:shadow-lg"
                    >
                        <div class="aspect-w-12 aspect-h-9 mb-2">
                            <img
                                :src="model.image"
                                :alt="model.name"
                                class="w-full h-full object-cover rounded-lg"
                            />
                        </div>
                        <div class="text-center">
                            <h3 class="text-lg font-semibold text-gray-900">
                                {{ activeBrand }}
                            </h3>
                            <p class="text-gray-600">{{ model.name }}</p>
                        </div>
                    </div>
                </div>
            </section>

            <section class="mb-12">
                <p class="text-xl font-semibold text-blue-950 mb-6">
                    O que as pessoas
                    <span class="block md:inline">mais buscam</span>
                </p>

                <div
                    class="grid grid-cols-1 md:grid-cols-1 lg:grid-cols-2 gap-4"
                >
                    <!-- FIPE Card -->
                    <div
                        class="relative aspect-video rounded-lg overflow-hidden md:col-span-2 lg:col-span-1 group cursor-pointer"
                    >
                        <img
                            src="https://images.impresa.pt/sic/2024-06-13-salesman-handling-keys-to-customer/original"
                            alt="Carros em fila"
                            class="w-full h-full object-cover transition-transform duration-300 group-hover:scale-105"
                        />
                        <div
                            class="absolute inset-0 bg-gradient-to-t from-black/70 via-black/30 to-transparent p-6 flex flex-col justify-end"
                        >
                            <span class="text-white text-3xl font-bold mb-2"
                                >Alberto</span
                            >
                            <p class="text-white text-lg">
                                Afinal, quanto vale meu carro ou minha moto?
                            </p>
                        </div>
                    </div>

                    <!-- Search Cards Grid -->
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                        <div
                            v-for="card in searchCards"
                            :key="card.title"
                            class="relative col-span-1 aspect-video rounded-lg overflow-hidden group cursor-pointer"
                        >
                            <img
                                :src="card.image"
                                :alt="card.title"
                                class="w-full h-full object-cover transition-transform duration-300 group-hover:scale-105"
                            />
                            <div
                                class="absolute inset-0 bg-gradient-to-t from-black/50 via-black/25 to-transparent p-4 flex flex-col justify-end"
                            >
                                <p class="text-white text-lg">
                                    {{ card.title }}
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
            </section>

            <!-- Advertise Section -->
            <div class="grid md:grid-cols-2 lg:grid-cols-8 gap-8">
                <!-- Left Side - Benefits -->
                <div
                    class="md:col-span-3 lg:col-span-5 bg-blue-900 rounded-lg py-14 px-6 text-white"
                >
                    <div class="flex items-center justify-around gap-2 mb-6">
                        <h3 class="text-base lg:text-3xl font-semibold">
                            Anuncie agora na
                            <strong class="font-bold">Dn-Line</strong> <br />
                            o portal líder na geração de resultados
                        </h3>
                        <Megaphone class="w-12 h-12" />
                    </div>

                    <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
                        <div
                            v-for="benefit in benefits"
                            :key="benefit.title"
                            class="bg-blue-600/50 rounded-lg p-4 text-center cursor-pointer"
                        >
                            <h4 class="font-semibold text-lg mb-2">
                                {{ benefit.title }}
                            </h4>
                            <p class="text-sm text-white/90">
                                {{ benefit.description }}
                            </p>
                        </div>
                    </div>
                </div>

                <!-- Right Side - Action Buttons -->
                <div class="md:col-span-2 lg:col-span-3 flex flex-col gap-4">
                    <button
                        v-for="action in actions"
                        :key="action.title"
                        class="flex items-center justify-between p-4 border border-gray-200 rounded-lg hover:border-blue-900 hover:shadow-md transition-all duration-200"
                    >
                        <div class="flex items-center gap-4">
                            <component
                                :is="action.icon"
                                class="w-6 h-6 text-blue-600"
                            />
                            <div class="text-left">
                                <p class="text-sm font-medium text-gray-900">
                                    {{ action.title }}
                                </p>
                                <p class="text-xs text-gray-500">
                                    {{ action.subtitle }}
                                </p>
                            </div>
                        </div>
                        <ArrowRight class="w-5 h-5 text-blue-900" />
                    </button>
                </div>
            </div>
            <section class="py-12">
                <div class="max-w-7xl mx-auto">
                    <nav
                        class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6"
                    >
                        <!-- Properties for Sale -->
                        <div>
                            <h2
                                class="text-lg font-semibold text-blue-950 mb-4"
                            >
                                Imóveis à venda
                            </h2>
                            <ul class="space-y-2">
                                <li v-for="link in saleLinks" :key="link">
                                    <a
                                        :href="'#'"
                                        class="text-sm text-gray-600 hover:text-blue-600 hover:underline"
                                    >
                                        {{ link }}
                                    </a>
                                </li>
                            </ul>
                        </div>

                        <!-- Properties for Rent -->
                        <div>
                            <h2
                                class="text-lg font-semibold text-blue-950 mb-4"
                            >
                                Imóveis para alugar
                            </h2>
                            <ul class="space-y-2">
                                <li v-for="link in rentLinks" :key="link">
                                    <a
                                        :href="'#'"
                                        class="text-sm text-gray-600 hover:text-blue-600 hover:underline"
                                    >
                                        {{ link }}
                                    </a>
                                </li>
                            </ul>
                        </div>

                        <!-- Cars -->
                        <div>
                            <h2
                                class="text-lg font-semibold text-blue-950 mb-4"
                            >
                                Carros
                            </h2>
                            <ul class="space-y-2">
                                <li v-for="link in carLinks" :key="link">
                                    <a
                                        :href="'#'"
                                        class="text-sm text-gray-600 hover:text-blue-600 hover:underline"
                                    >
                                        {{ link }}
                                    </a>
                                </li>
                            </ul>
                        </div>

                        <!-- Cities -->
                        <div>
                            <h2
                                class="text-lg font-semibold text-blue-950 mb-4"
                            >
                                Cidades
                            </h2>
                            <ul class="space-y-2">
                                <li v-for="cidade in cidades" :key="cidade">
                                    <a
                                        :href="'#'"
                                        class="text-sm text-gray-600 hover:text-blue-600 hover:underline"
                                    >
                                        {{ cidade }}
                                    </a>
                                </li>
                            </ul>
                        </div>
                    </nav>
                </div>
            </section>
        </main>
    </div>
</template>

<script setup>
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
const saleLinks = [
    "Imóveis novos em São Paulo",
    "Imóveis novos em Curitiba",
    "Apartamentos à venda em Balneário Camboriú",
    "Apartamentos à venda em Ribeirão Preto",
    "Apartamentos à venda em Santo André",
    "Apartamentos à venda em Joinville",
    "Apartamentos à venda em Londrina",
    "Apartamentos à venda em São José dos Campos",
    "Apartamentos à venda em Santos",
    "Apartamentos à venda em Campinas",
];

const rentLinks = [
    "Apartamentos para alugar em Balneário Camboriú",
    "Apartamentos para alugar em Ribeirão Preto",
    "Apartamentos para alugar em Santo André",
    "Apartamentos para alugar em Joinville",
    "Apartamentos para alugar em Londrina",
    "Apartamentos para alugar em São José dos Campos",
    "Apartamentos para alugar em Santos",
    "Apartamentos para alugar em Itapema",
    "Casas para alugar em Ribeirão Preto",
];

const cidades = [
    "Campinas",
    "Balneário Camboriú",
    "Joinville",
    "Londrina",
    "Ribeirão Preto",
    "São José dos Campos",
    "Santos",
    "Santo André",
    "Itapema",
    "Guarulhos",
];

const carLinks = [
    "Carros antigos",
    "Motos à venda",
    "Carros em São Paulo",
    "Carros no Rio de Janeiro",
    "Carros em Curitiba",
    "Carros em Porto Alegre",
    "Carros em Florianópolis",
    "Carros em Ribeirão Preto",
    "Carros em Santo André",
    "Carros em Joinville",
    "Carros em Londrina",
];

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

const benefits = [
    {
        title: "Mais visibilidade",
        description: "Contamos com milhões de acessos todos os meses",
    },
    {
        title: "Mais oportunidades",
        description: "Aumente hoje suas chances de venda",
    },
    {
        title: "Mais resultados",
        description: "Somos o portal líder na geração de resultados",
    },
];

const actions = [
    {
        title: "Corretores, imobiliárias",
        subtitle: "e incorporadoras",
        icon: Building2,
    },
    {
        title: "Revendas de carros, motos",
        subtitle: "e concessionárias",
        icon: Car,
    },
    {
        title: "Particular, anuncie",
        subtitle: "seu imóvel ou veículo",
        icon: User,
    },
];

const searchTags = [
    "Apartamentos à venda",
    "Cobertura à venda",
    "Apartamentos para alugar",
    "Sala comercial para alugar",
    "Ponto comercial à venda",
    "Terreno em condomínio fechado",
    "Barracão para alugar",
    "Kitnet à venda",
    "Cobertura à venda",
    "Ponto comercial à venda",
    "Casa comercial para alugar",
    "Sala comercial à venda",
    "Cobertura para alugar",
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

// Lista de modelos de carros
const carModels = [
    {
        brand: "Chevrolet",
        name: "Prisma",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/chevrolet-prisma.jpg",
    },
    {
        brand: "Chevrolet",
        name: "Onix",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/chevrolet-onix.jpg",
    },
    {
        brand: "Chevrolet",
        name: "S10",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/chevrolet-s10.jpg",
    },
    {
        brand: "Chevrolet",
        name: "Cruze",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/chevrolet-cruze.jpg",
    },
    {
        brand: "Fiat",
        name: "Palio",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/fiat-palio.jpg",
    },
    {
        brand: "Fiat",
        name: "Siena",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/fiat-siena.jpg",
    },
    {
        brand: "Fiat",
        name: "Strada",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/fiat-strada.jpg",
    },
    {
        brand: "Fiat",
        name: "Uno",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/fiat-uno.jpg",
    },
    {
        brand: "Ford",
        name: "EcoSport",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/ford-ecosport.jpg",
    },
    {
        brand: "Ford",
        name: "Focus",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/ford-focus.jpg",
    },
    {
        brand: "Ford",
        name: "Ka",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/ford-ka.jpg",
    },
    {
        brand: "Ford",
        name: "Ranger",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/ford-ranger.jpg",
    },

    {
        brand: "Honda",
        name: "Civic",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/honda-civic.jpg",
    },
    {
        brand: "Honda",
        name: "CR-V",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/honda-crv.jpg",
    },
    {
        brand: "Honda",
        name: "Fit",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/honda-fit.jpg",
    },
    {
        brand: "Honda",
        name: "City",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/honda-city.jpg",
    },
    {
        brand: "Hyundai",
        name: "HB20",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/hyundai-hb20.jpg",
    },
    {
        brand: "Hyundai",
        name: "HB20s",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/hyundai-hb20s.jpg",
    },
    {
        brand: "Hyundai",
        name: "IX35",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/hyundai-ix35.jpg",
    },
    {
        brand: "Hyundai",
        name: "Tucson",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/hyundai-tucson.jpg",
    },
    {
        brand: "Renault",
        name: "Duster",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/renault-duster.jpg",
    },
    {
        brand: "Renault",
        name: "Kwid",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/renault-kwid.jpg",
    },
    {
        brand: "Renault",
        name: "Logan",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/renault-logan.jpg",
    },
    {
        brand: "Renault",
        name: "Sandero",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/renault-sandero.jpg",
    },
    {
        brand: "Volkswagen",
        name: "Jetta",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/volkswagen-jetta.jpg",
    },
    {
        brand: "Volkswagen",
        name: "Fox",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/volkswagen-fox.jpg",
    },
    {
        brand: "Volkswagen",
        name: "Gol",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/gol.jpg",
    },
    {
        brand: "Volkswagen",
        name: "Saveiro",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/volkswagen-saveiro.jpg",
    },
    {
        brand: "Toyota",
        name: "Corlla",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/toyota-corolla.jpg",
    },
    {
        brand: "Toyota",
        name: "Etios",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/toyota-etios.jpg",
    },
    {
        brand: "Toyota",
        name: "Hilux",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/toyota-hilux.jpg",
    },
    {
        brand: "Toyota",
        name: "Hilux SW4",
        image: "https://cdn.chavesnamao.com.br/portal/cards/vehicle/toyota-hilux-sw4.jpg",
    },
];

const categoriesCity = [
    {
        icon: BuildingIcon,
        title: "Imóveis para alugar",
        subtitle: "Encontre seu lar",
    },
    {
        icon: HomeIcon,

        title: "Imóveis à venda",
        subtitle: "Realize seu sonho",
    },
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

const cities = [
    {
        name: "Curitiba",
        image: "https://cdn.chavesnamao.com.br/portal/capitals/Home-Imagens-Cidades-curitiba.jpg",
        links: [
            "Apartamentos à venda em Curitiba",
            "Casas para alugar em Curitiba",
        ],
    },
    {
        name: "São Paulo",
        image: "https://cdn.chavesnamao.com.br/portal/capitals/Home-Imagens-Cidades-sao-paulo.jpg",
        links: [
            "Apartamentos para alugar em São Paulo",
            "Casas para alugar em São Paulo",
        ],
    },
    {
        name: "Rio de Janeiro",
        image: "https://cdn.chavesnamao.com.br/portal/capitals/Home-Imagens-Cidades-salvador.jpg",
        links: [
            "Apartamentos à venda em Rio de Janeiro",
            "Casas para alugar em Rio de Janeiro",
        ],
    },
    {
        name: "Porto Alegre",
        image: "https://cdn.chavesnamao.com.br/portal/capitals/Home-Imagens-Cidades-porto-alegre.jpg",
        links: [
            "Apartamentos à venda em Porto Alegre",
            "Casas para alugar em Porto Alegre",
        ],
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
