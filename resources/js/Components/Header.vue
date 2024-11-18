<template>
    <header class="relative">
        <!-- Hero Image Section with Slider -->
        <div
            class="relative h-[300px] sm:h-[400px] md:h-[500px] lg:h-[600px] overflow-hidden"
        >
            <transition-group name="fade">
                <img
                    v-for="(image, index) in images"
                    :key="image"
                    :src="image"
                    :alt="`Slide ${index + 1}`"
                    class="w-full h-full object-cover absolute top-0 left-0"
                    v-show="currentIndex === index"
                />
            </transition-group>

            <!-- Navigation arrows -->
            <button
                @click="prevSlide"
                class="absolute left-2 sm:left-4 top-1/2 transform -translate-y-1/2 bg-black bg-opacity-50 text-white p-1 sm:p-2 rounded-full"
            >
                <ChevronLeftIcon class="w-4 h-4 sm:w-6 sm:h-6" />
            </button>
            <button
                @click="nextSlide"
                class="absolute right-2 sm:right-4 top-1/2 transform -translate-y-1/2 bg-black bg-opacity-50 text-white p-1 sm:p-2 rounded-full"
            >
                <ChevronRightIcon class="w-4 h-4 sm:w-6 sm:h-6" />
            </button>

            <!-- Slide indicators -->
            <div
                class="absolute bottom-2 sm:bottom-4 left-1/2 transform -translate-x-1/2 flex space-x-1 sm:space-x-2"
            >
                <button
                    v-for="(_, index) in images"
                    :key="index"
                    @click="currentIndex = index"
                    class="w-2 h-2 sm:w-3 sm:h-3 rounded-full"
                    :class="currentIndex === index ? 'bg-white' : 'bg-white'"
                ></button>
            </div>
        </div>

        <!-- Navigation Section -->
        <nav class="bg-gray-900 text-white">
            <div class="container mx-auto sm:px-4">
                <!-- Menu Items -->
                <div class="flex flex-wrap border-t border-gray-800">
                    <button
                        class="w-1/2 sm:w-auto px-3 sm:px-6 py-3 sm:py-4 bg-gray-700 text-white text-xs sm:text-sm font-medium hover:bg-gray-700 transition-colors"
                    >
                        CARROS NOVOS
                    </button>
                    <button
                        class="w-1/2 sm:w-auto px-3 sm:px-6 py-3 sm:py-4 text-gray-300 text-xs sm:text-sm hover:bg-gray-700 transition-colors"
                    >
                        CARROS USADOS
                    </button>
                    <button
                        class="w-1/2 sm:w-auto px-3 sm:px-6 py-3 sm:py-4 text-gray-300 text-xs sm:text-sm hover:bg-gray-700 transition-colors"
                    >
                        VENDER CARRO
                    </button>
                    <button
                        class="w-1/2 sm:w-auto px-3 sm:px-6 py-3 sm:py-4 text-gray-300 text-xs sm:text-sm hover:bg-gray-700 transition-colors"
                    >
                        BUSCAR
                    </button>
                </div>
            </div>
        </nav>
    </header>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from "vue";
import { ChevronLeftIcon, ChevronRightIcon } from "lucide-vue-next";

const images = [
    "https://es.valleychevy.com/wp-content/uploads/2022/01/Chevy-Trax.png",
    "https://www.pngitem.com/pimgs/m/241-2415516_2019-chevrolet-trax-2019-chevy-trax-hd-png.png",
    "https://ymimg1.b8cdn.com/resized/car_model/4538/pictures/4020419/listing_main_01.jpg",
];

const currentIndex = ref(0);

const nextSlide = () => {
    currentIndex.value = (currentIndex.value + 1) % images.length;
};

const prevSlide = () => {
    currentIndex.value =
        (currentIndex.value - 1 + images.length) % images.length;
};

let intervalId;

onMounted(() => {
    // Auto-advance slides every 5 seconds
    intervalId = setInterval(nextSlide, 5000);
});

onBeforeUnmount(() => {
    clearInterval(intervalId);
});
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}
</style>
