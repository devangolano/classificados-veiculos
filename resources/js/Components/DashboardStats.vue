<template>
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 mb-8">
        <div
            v-for="stat in stats"
            :key="stat.name"
            class="bg-white overflow-hidden shadow rounded-lg"
        >
            <div class="p-5">
                <div class="flex items-center">
                    <div class="flex-shrink-0">
                        <component
                            :is="stat.icon"
                            class="h-6 w-6 text-gray-400"
                        />
                    </div>
                    <div class="ml-5 w-0 flex-1">
                        <dl>
                            <dt
                                class="text-sm font-medium text-gray-500 truncate"
                            >
                                {{ stat.name }}
                            </dt>
                            <dd class="flex items-baseline">
                                <div
                                    class="text-2xl font-semibold text-gray-900"
                                >
                                    {{ stat.value }}
                                </div>
                            </dd>
                        </dl>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { Users, DollarSign, ShoppingCart, TrendingUp } from "lucide-vue-next";

const stats = [
    { name: "Total Users", value: "1,234", icon: Users },
    { name: "Total Revenue", value: "$12,345", icon: DollarSign },
    { name: "Total Orders", value: "567", icon: ShoppingCart },
    { name: "Growth", value: "12%", icon: TrendingUp },
];
</script>
