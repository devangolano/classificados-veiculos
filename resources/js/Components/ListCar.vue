<template>
    <div class="container mx-auto">
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
            <div
                v-for="car in currentItems"
                :key="car.id"
                class="bg-white cursor-pointer rounded-lg shadow-lg overflow-hidden relative group"
            >
                <!-- Special Badge -->
                <div
                    v-if="car.isNew"
                    class="absolute top-4 left-[-35px] rotate-[-45deg] bg-blue-500 text-white px-10 py-1 transform z-10"
                >
                    <span class="text-sm font-bold">NOVO</span>
                </div>

                <!-- Car Images with Hover Effect -->
                <div
                    class="relative h-48 overflow-hidden"
                    @mouseenter="isHovered[car.id] = true"
                    @mouseleave="isHovered[car.id] = false"
                >
                    <img
                        :src="car.image"
                        :alt="car.name"
                        class="w-full h-full object-cover transition-opacity duration-300 ease-in-out"
                        :class="{ 'opacity-0': isHovered[car.id] }"
                    />
                    <img
                        :src="car.hoverImage"
                        :alt="car.name + ' (alternate view)'"
                        class="w-full h-full object-cover absolute top-0 left-0 transition-opacity duration-300 ease-in-out"
                        :class="{
                            'opacity-100': isHovered[car.id],
                            'opacity-0': !isHovered[car.id],
                        }"
                    />
                </div>

                <!-- Car Details -->
                <div class="p-4">
                    <h3 class="text-lg font-bold text-gray-900">
                        {{ car.name }}
                    </h3>
                    <div class="flex items-center mt-2">
                        <span class="line-through text-gray-400 text-sm"
                            >${{ car.originalPrice.toLocaleString() }}</span
                        >
                        <span class="ml-2 text-blue-500 font-bold"
                            >${{ car.price.toLocaleString() }}</span
                        >
                    </div>

                    <!-- Stats -->
                    <div
                        class="flex items-center justify-between mt-4 text-sm text-gray-600"
                    >
                        <div class="flex items-center">
                            <GaugeIcon class="w-4 h-4 mr-1" />
                            <span>{{ car.mileage }}/{{ car.maxMileage }}</span>
                        </div>
                        <div class="flex items-center">
                            <Settings2Icon class="w-4 h-4 mr-1" />
                            <span>{{ car.transmission }}</span>
                        </div>
                        <div class="flex items-center">
                            <CarIcon class="w-4 h-4 mr-1" />
                            <span>{{ car.usage }}Km</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Pagination -->
        <div class="mt-8 flex justify-center">
            <nav class="flex items-center gap-2">
                <button
                    @click="previousPage"
                    :disabled="currentPage === 1"
                    class="p-2 rounded-lg border enabled:hover:bg-gray-100 disabled:opacity-50"
                >
                    <ChevronLeftIcon class="w-5 h-5" />
                </button>

                <div class="flex gap-1">
                    <button
                        v-for="page in totalPages"
                        :key="page"
                        @click="goToPage(page)"
                        class="px-4 py-2 rounded-lg"
                        :class="
                            currentPage === page
                                ? 'bg-blue-500 text-white'
                                : 'hover:bg-gray-100'
                        "
                    >
                        {{ page }}
                    </button>
                </div>

                <button
                    @click="nextPage"
                    :disabled="currentPage === totalPages"
                    class="p-2 rounded-lg border enabled:hover:bg-gray-100 disabled:opacity-50"
                >
                    <ChevronRightIcon class="w-5 h-5" />
                </button>
            </nav>
        </div>
    </div>
</template>

<script setup>
import { ref, computed } from "vue";
import {
    GaugeIcon,
    Settings2Icon,
    CarIcon,
    ChevronLeftIcon,
    ChevronRightIcon,
} from "lucide-vue-next";

// Sample data
const cars = ref([
    {
        id: 1,
        name: "MERCEDES-BENZ C-CLASS 2015",
        image: "https://motors.stylemixthemes.com/elementor-dealer-one/wp-content/uploads/sites/22/2015/10/motor-1-795x463.jpg",
        hoverImage:
            "https://motors.stylemixthemes.com/elementor-dealer-one/wp-content/uploads/sites/22/2015/10/5-1109x699.jpg",
        originalPrice: 39500,
        price: 35000,
        mileage: "18",
        maxMileage: "26",
        transmission: "Automatico",
        usage: "100",
        isNew: true,
    },
    {
        id: 2,
        name: "TESLA ROADSTER 2021",
        image: "https://www.iihs.org/cdn-cgi/image/width=636/api/ratings/model-year-images/2421/",
        hoverImage:
            "https://vehicle-images.dealerinspire.com/0fb1-110008861/W1KWF8EB9MR605885/004c9590ca7cf78368926abd974fb623.jpg",
        originalPrice: 121000,
        price: 109000,
        mileage: "18",
        maxMileage: "100",
        transmission: "Automatico",
        usage: "400",
        isNew: false,
    },
    {
        id: 3,
        name: "NISSAN ALTIMA 2019",
        image: "https://i.1.creatium.io/dc/5b/40/d40d0baae1caa29d7a7b51eb06b3d5499e/mercedes_benz_c_class_3_pokolenie_w204_03_2007_02_2015.jpg",
        hoverImage:
            "https://afxfuvomxq.cloudimg.io/v7/https://www.autoplenum.de/rails/active_storage/blobs/redirect/eyJfcmFpbHMiOnsibWVzc2FnZSI6IkJBaHBBelZ3QVE9PSIsImV4cCI6bnVsbCwicHVyIjoiYmxvYl9pZCJ9fQ==--2b333f2b7ebe05c7c77796eeddeab5533df18b9a/mercedes-benz-c-klasse-limousine-2007-seitlich-vorne-1171445.jpg",
        originalPrice: 25000,
        price: 18000,
        mileage: "25",
        maxMileage: "36",
        transmission: "Automatico",
        usage: "18000",
        isNew: true,
    },
    {
        id: 4,
        name: "TESLA ROADSTER 2021",
        image: "https://www.indystar.com/gcdn/-mm-/845b6e54f2b5a851361b9382d3239722a2eb3a37/c=137-308-862-717/local/-/media/2015/03/07/Indianapolis/Indianapolis/635613411256723799-2015-C-Class-8-.jpg?width=660&height=373&fit=crop&format=pjpg&auto=webp",
        hoverImage:
            "https://upload.wikimedia.org/wikipedia/commons/thumb/5/52/Mercedes-Benz_C_200_Avantgarde_%28W_205%29_%E2%80%93_Frontansicht%2C_26._April_2014%2C_D%C3%BCsseldorf.jpg/1200px-Mercedes-Benz_C_200_Avantgarde_%28W_205%29_%E2%80%93_Frontansicht%2C_26._April_2014%2C_D%C3%BCsseldorf.jpg",
        originalPrice: 121000,
        price: 109000,
        mileage: "18",
        maxMileage: "100",
        transmission: "Automatico",
        usage: "400",
        isNew: false,
    },
    // Add more cars as needed...
]);

// Hover state
const isHovered = ref({});

// Pagination
const itemsPerPage = 8;
const currentPage = ref(1);

const totalPages = computed(() => Math.ceil(cars.value.length / itemsPerPage));

const currentItems = computed(() => {
    const start = (currentPage.value - 1) * itemsPerPage;
    const end = start + itemsPerPage;
    return cars.value.slice(start, end);
});

const previousPage = () => {
    if (currentPage.value > 1) {
        currentPage.value--;
    }
};

const nextPage = () => {
    if (currentPage.value < totalPages.value) {
        currentPage.value++;
    }
};

const goToPage = (page) => {
    currentPage.value = page;
};
</script>
