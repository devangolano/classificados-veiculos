<template>
    <div class="bg-white shadow rounded-lg p-4">
        <h2 class="text-lg font-semibold mb-4">Recent Activities</h2>
        <ul class="divide-y divide-gray-200">
            <li v-for="activity in activities" :key="activity.id" class="py-4">
                <div class="flex space-x-3">
                    <img
                        class="h-6 w-6 rounded-full"
                        :src="activity.userImageUrl"
                        alt=""
                    />
                    <div class="flex-1 space-y-1">
                        <div class="flex items-center justify-between">
                            <h3 class="text-sm font-medium">
                                {{ activity.user }}
                            </h3>
                            <p class="text-sm text-gray-500">
                                {{ activity.time }}
                            </p>
                        </div>
                        <p class="text-sm text-gray-500">
                            {{ activity.description }}
                        </p>
                    </div>
                </div>
            </li>
        </ul>
    </div>
</template>

<script setup>
const activities = [
    {
        id: 1,
        user: "John Doe",
        userImageUrl:
            "https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80",
        time: "2 hours ago",
        description: "Completed a new order #12345",
    },
    {
        id: 2,
        user: "Jane Smith",
        userImageUrl:
            "https://images.unsplash.com/photo-1494790108377-be9c29b29330?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80",
        time: "4 hours ago",
        description: "Updated product inventory",
    },
    {
        id: 3,
        user: "Bob Johnson",
        userImageUrl:
            "https://images.unsplash.com/photo-1519244703995-f4e0f30006d5?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80",
        time: "Yesterday",
        description: "Resolved customer support ticket #5678",
    },
];
</script>
