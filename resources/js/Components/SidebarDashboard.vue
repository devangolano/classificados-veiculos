<template>
    <aside class="bg-gray-800 text-white w-64 min-h-screen p-4">
        <nav>
            <ul>
                <li v-for="item in menuItems" :key="item.name" class="mb-2">
                    <button
                        @click="toggleSubmenu(item)"
                        class="flex items-center justify-between w-full p-2 rounded-lg hover:bg-gray-700 transition-colors duration-200"
                    >
                        <div class="flex items-center">
                            <component :is="item.icon" class="h-6 w-6 mr-3" />
                            {{ item.name }}
                        </div>
                        <ChevronDown
                            v-if="item.submenu"
                            :class="{ 'transform rotate-180': item.isOpen }"
                            class="h-4 w-4 transition-transform duration-200"
                        />
                    </button>
                    <transition
                        enter-active-class="transition-all duration-300 ease-in-out"
                        enter-from-class="opacity-0 max-h-0"
                        enter-to-class="opacity-100 max-h-96"
                        leave-active-class="transition-all duration-300 ease-in-out"
                        leave-from-class="opacity-100 max-h-96"
                        leave-to-class="opacity-0 max-h-0"
                    >
                        <ul
                            v-if="item.submenu && item.isOpen"
                            class="ml-6 mt-2 space-y-2"
                        >
                            <li
                                v-for="subItem in item.submenu"
                                :key="subItem.name"
                            >
                                <a
                                    @click="selectMenuItem(subItem)"
                                    href="#"
                                    class="block p-2 rounded-lg hover:bg-gray-700 transition-colors duration-200"
                                >
                                    {{ subItem.name }}
                                </a>
                            </li>
                        </ul>
                    </transition>
                </li>
            </ul>
        </nav>
    </aside>
</template>

<script setup>
import { ref } from "vue";
import { Home, BarChart2, Users, Settings, ChevronDown } from "lucide-vue-next";

const menuItems = ref([
    {
        name: "Principal",
        icon: Home,
        submenu: [
            { name: "Dashboard", component: "DashboardContent" },
            { name: "Análises", component: "AnalyticsContent" },
        ],
    },
    {
        name: "Usuarios",
        icon: Users,
        submenu: [
            { name: "Cadastrar Usuários", component: "UserStatsContent" },
            { name: "Listar Usuarios", component: "UserListContent" },
        ],
    },
    {
        name: "Relatórios",
        icon: BarChart2,
        submenu: [
            { name: "Listar Relatórios", component: "SalesReportContent" },
        ],
    },
    {
        name: "Definições",
        icon: Settings,
        submenu: [
            { name: "Definições Gerais", component: "GeneralSettingsContent" },
            {
                name: "Definições de segurança",
                component: "SecuritySettingsContent",
            },
        ],
    },
]);

const toggleSubmenu = (item) => {
    item.isOpen = !item.isOpen;
};

const emit = defineEmits(["select-menu-item"]);

const selectMenuItem = (subItem) => {
    emit("select-menu-item", subItem.component);
};
</script>
