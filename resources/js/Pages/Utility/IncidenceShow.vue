<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import { Head } from '@inertiajs/vue3';

const props = defineProps ({
    customroute: {
        type: String,
        default: () => "utility.incidence.show",
    },
    incidences: {
        type: Array,
        default: () => [],
    },
});

console.log(props.incidences);

</script>

<template>
    <Head title="Útiles" />

    <AuthenticatedLayout :croute="customroute">

        <div class="mx-auto">
            Incidencias
        </div>

    </AuthenticatedLayout>
</template>

<style scoped>

</style>