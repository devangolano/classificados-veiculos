<script setup lang="ts">
import AppLayout from "@/Layouts/AppLayout.vue";
</script>

<!-- <template>
    <div class="flex h-screen bg-gray-100">
        <SidebarDashboard @select-menu-item="changeContent" />
        <div class="flex-1 flex flex-col overflow-hidden">
            <HeaderDashboard />
            <main class="flex-1 overflow-x-hidden overflow-y-auto bg-gray-100">
                <div class="container mx-auto px-6 py-8">
                    <component :is="currentComponent" />
                </div>
            </main>
        </div>
    </div>
</template>

<script setup>
import { ref, shallowRef } from "vue";
import HeaderDashboard from "@/Components/HeaderDashboard.vue";
import SidebarDashboard from "@/Components/SidebarDashboard.vue";
import DashboardContent from "@/Components/DashboardContent.vue";
import UserListContent from "@/Components/UserListContent.vue";

const currentComponent = shallowRef(DashboardContent);

const changeContent = (componentName) => {
    switch (componentName) {
        case "DashboardContent":
            currentComponent.value = DashboardContent;
            break;
        case "UserListContent":
            currentComponent.value = UserListContent;
            break;
        default:
            currentComponent.value = DashboardContent;
    }
};
</script> -->
<template>
    <AppLayout> </AppLayout>
</template>
