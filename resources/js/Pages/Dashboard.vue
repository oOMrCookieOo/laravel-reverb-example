<script setup lang="ts">
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import {Head, usePage} from '@inertiajs/vue3';
import {onMounted} from "vue";


onMounted(() => {
    const user = usePage().props.auth.user
    //@ts-ignore
    window.Echo.private(`App.Models.User.${user.id}`)
        .listen('ReverbExampleEvent', (e:any) => {
           alert('Reverb 🚀')
        })

});
</script>

<template>
    <Head title="Dashboard" />
    <AuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 dark:text-gray-200 leading-tight">Dashboard</h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white dark:bg-gray-800 overflow-hidden shadow-sm sm:rounded-lg">
                    <div class="p-6 text-gray-900 dark:text-gray-100">You're logged in!</div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
