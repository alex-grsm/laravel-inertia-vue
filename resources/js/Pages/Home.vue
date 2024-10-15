<template>
    <Head :title="` | {$page.component}`" />

    <!-- <h1>Home page</h1> -->

    <div class="max-w-4xl mx-auto bg-white mt-6">
        <div class="flex justify-end mb-4">
            <div class="w-1/4">
                <input type="search" placeholder="Search" v-model="search" />
            </div>
        </div>

        <table class="min-w-full bg-white">
            <thead>
                <tr
                    class="w-full bg-gray-200 text-left text-gray-600 text-sm uppercase font-bold tracking-wider"
                >
                    <th class="py-3 px-4">Avatar</th>
                    <th class="py-3 px-4">Name</th>
                    <th class="py-3 px-4">Email</th>
                    <th class="py-3 px-4">Registration Date</th>
                    <th v-if="can.delete_user">Delete</th>
                </tr>
            </thead>
            <tbody class="text-gray-700">
                <tr class="border-b" v-for="user in users.data" :key="user.id">
                    <td class="py-4 px-4">
                        <img
                            :src="
                                user.avatar
                                    ? 'storage/' + user.avatar
                                    : 'storage/avatars/default.png'
                            "
                            alt="Avatar"
                            class="rounded-full w-10 h-10"
                        />
                    </td>
                    <td class="py-4 px-4">
                        <span class="text-sm font-medium">{{ user.name }}</span>
                    </td>
                    <td class="py-4 px-4">
                        <span class="text-sm">{{ user.email }}</span>
                    </td>
                    <td class="py-4 px-4">
                        <span class="text-sm">{{
                            getDate(user.created_at)
                        }}</span>
                    </td>
                    <td v-if="can.delete_user">
                        <button class="bg-red-500 w-6 h-6 rounded-full"></button>
                    </td>
                </tr>
            </tbody>
        </table>

        <!-- Pagination Links -->
        <div>
            <PaginationLinks :paginator="users" />
        </div>
    </div>
</template>

<script setup>
import { ref, watch } from "vue";
import PaginationLinks from "@/Components/PaginationLinks.vue";
import { router } from "@inertiajs/vue3";
import { debounce } from "lodash";

const props = defineProps({
    users: Object,
    searchTerm: String,
    can: Object,
});

const search = ref(props.searchTerm);

watch(
    search,
    debounce((searchQuery) => router.get("/", { search: searchQuery }, { preserveState: true }), 500)
);

// Format date
const getDate = (date) => {
    return new Date(date).toLocaleDateString("en-us", {
        year: "numeric",
        month: "long",
        day: "numeric",
    });
};
</script>
