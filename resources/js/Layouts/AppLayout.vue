<template>
    <div class="fixed w-full bg-slate-50 z-50">
        <nav
            class="flex items-center justify-between px-6 lg:px-16 py-4 shadow-md shadow-blue-950/30"
        >
            <!-- Mobile: Logo de um lado e Menu Hamburguer do outro -->
            <div class="flex items-center justify-between w-full md:hidden">
                <div>
                    <Link href="/"
                        ><img
                            src="logo.png"
                            alt="Logo"
                            class="h-14 cursor-pointer"
                    /></Link>
                </div>
                <div>
                    <button
                        @click="toggleMenu"
                        class="text-black focus:outline-none"
                    >
                        <svg
                            xmlns="http://www.w3.org/2000/svg"
                            fill="none"
                            viewBox="0 0 24 24"
                            stroke="currentColor"
                            class="w-10 h-10"
                        >
                            <path
                                stroke-linecap="round"
                                stroke-linejoin="round"
                                stroke-width="2"
                                d="M4 6h16M4 12h16M4 18h16"
                            />
                        </svg>
                    </button>
                </div>
            </div>

            <!-- Tablet: Menu Hamburguer no meio, Logo no centro e Entrar no fim -->
            <div
                class="hidden md:flex lg:hidden justify-between items-center w-full"
            >
                <div>
                    <button
                        @click="toggleMenu"
                        class="text-black focus:outline-none"
                    >
                        <svg
                            xmlns="http://www.w3.org/2000/svg"
                            fill="none"
                            viewBox="0 0 24 24"
                            stroke="currentColor"
                            class="w-10 h-10"
                        >
                            <path
                                stroke-linecap="round"
                                stroke-linejoin="round"
                                stroke-width="2"
                                d="M4 6h16M4 12h16M4 18h16"
                            />
                        </svg>
                    </button>
                </div>
                <div>
                    <Link href="/"
                        ><img
                            src="logo.png"
                            alt="Logo"
                            class="h-14 cursor-pointer"
                    /></Link>
                </div>
                <div>
                    <Link href="/login"
                        ><button
                            class="px-3 py-1 border-blue-900 cursor-pointer border rounded-lg"
                        >
                            Entrar
                        </button></Link
                    >
                </div>
            </div>

            <!-- Desktop: Menu Horizontal -->
            <div
                class="hidden lg:flex items-center justify-between w-full gap-14"
            >
                <!-- Logo no lado esquerdo -->
                <div class="flex items-center justify-start">
                    <Link href="/"
                        ><img
                            src="logo.png"
                            alt="Logo"
                            class="h-14 cursor-pointer"
                    /></Link>
                </div>

                <!-- Itens do Menu no centro -->
                <ul class="flex gap-6 text-black">
                    <li class="cursor-pointer relative">
                        <p
                            class="border-b-2 flex items-center gap-2 border-transparent hover:border-blue-900 pb-1"
                        >
                            <Home class="w-6 h-6 text-blue-900" />
                            Página Inicial
                        </p>
                    </li>
                    <li class="cursor-pointer relative">
                        <p
                            class="border-b-2 flex items-center gap-2 border-transparent hover:border-blue-900 pb-1"
                        >
                            <Car class="w-6 h-6 text-blue-900" />
                            Comprar Veículo
                        </p>
                    </li>
                    <li class="cursor-pointer relative">
                        <p
                            class="border-b-2 flex items-center gap-2 border-transparent hover:border-blue-900 pb-1"
                        >
                            <Car class="w-6 h-6 text-blue-900" />
                            Vender Veículo
                        </p>
                    </li>
                    <li class="cursor-pointer relative">
                        <p
                            class="border-b-2 flex items-center gap-2 border-transparent hover:border-blue-900 pb-1"
                        >
                            <Megaphone class="w-6 h-6 text-blue-900" />
                            Anunciar Agora!
                        </p>
                    </li>
                </ul>

                <!-- Botão Entrar no lado direito -->
                <div class="flex items-center justify-end">
                    <Link href="/login"
                        ><button
                            class="px-3 py-1 border-blue-900 cursor-pointer border rounded-lg"
                        >
                            Entrar
                        </button></Link
                    >
                </div>
            </div>
        </nav>

        <!-- Menu mobile e tablet -->
        <div
            v-if="isMenuOpen"
            class="absolute lg:hidden top-16 left-0 mt-3 w-full bg-white shadow-lg p-4 z-50 transform transition-opacity duration-500 ease-in-out opacity-0 translate-y-4 md:block"
            :class="{ 'opacity-100 translate-y-0': isMenuOpen }"
        >
            <ul class="flex flex-col gap-2 justify-center text-black">
                <li
                    class="cursor-pointer transition-all duration-500 ease-in-out"
                    :style="{ transitionDelay: '0ms' }"
                >
                    <Link href="/"
                        ><span
                            class="hover:border-b-2 flex items-center gap-2 border-transparent hover:border-blue-900"
                        >
                            <Home class="w-5 h-5 text-blue-900" />
                            Página Inicial
                        </span></Link
                    >
                </li>
                <li
                    class="cursor-pointer transition-all duration-500 ease-in-out"
                    :style="{ transitionDelay: '100ms' }"
                >
                    <span
                        class="hover:border-b-2 flex items-center gap-2 border-transparent hover:border-blue-900"
                    >
                        <Car class="w-5 h-5 text-blue-900" />
                        Comprar Veículo
                    </span>
                </li>

                <li
                    class="cursor-pointer transition-all duration-500 ease-in-out"
                    :style="{ transitionDelay: '100ms' }"
                >
                    <span
                        class="hover:border-b-2 flex items-center gap-2 border-transparent hover:border-blue-900"
                    >
                        <Car class="w-5 h-5 text-blue-900" />
                        Vender Veículo
                    </span>
                </li>
                <li
                    class="cursor-pointer transition-all duration-500 ease-in-out"
                    :style="{ transitionDelay: '300ms' }"
                >
                    <span
                        class="hover:border-b-2 flex items-center gap-2 border-transparent hover:border-blue-900"
                    >
                        <Megaphone class="w-5 h-5 text-blue-900" />
                        Anunciar Agora!
                    </span>
                </li>
            </ul>
            <Link href="/login">
                <button
                    class="w-full px-3 py-1 mt-4 border-blue-900 cursor-pointer border rounded-lg"
                >
                    Entrar
                </button>
            </Link>
        </div>
    </div>
    <main class="pt-20 px-1"><slot /></main>
</template>

<script setup>
import { ref } from "vue";
import { Link } from "@inertiajs/vue3";
import { Car, Home, Hotel, Megaphone } from "lucide-vue-next";

const isMenuOpen = ref(false);

// Função para alternar o estado do menu
const toggleMenu = () => {
    isMenuOpen.value = !isMenuOpen.value;
};
</script>
