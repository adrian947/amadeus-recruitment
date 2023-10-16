<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import { Head } from '@inertiajs/vue3';
import GroupElementTarget from '@/Pages/Inventory/Partials/GroupElementTarget.vue';
import Modal from '@/Components/Amadeus/Modal.vue';
import ButtonOutline from '@/Components/ButtonOutline.vue';
import ButtonGeneric from '@/Components/ButtonGeneric.vue';

const props = defineProps ({
    customroute: {
        type: String,
        default: () => "inventory.show",
    },
    items: {
        type: Array,
        default: () => [],
    },
    loan_agreement: {
        type: String,
        default: () => "",
    }
});

</script>

<template>
    <Head title="Inventario" />

    <AuthenticatedLayout :croute="customroute">
        <div class="mx-auto">
            Lista de inventario
            <div v-for="item in items" :key="item.id">
                <GroupElementTarget :obj="item" />                
            </div>
        </div>
        <div 
            class="position-fixed bottom-12 right-12 p-4 rounded-full d-flex items-center justify-center" 
            style="background: #3c7ff8" type="button" data-bs-toggle="modal" data-bs-target="#exampleModal"
        >
            <i class="fa-solid fa-plus text-white text-2xl position-absolute"></i>
        </div>

    </AuthenticatedLayout>
    <Modal maxWidth="60rem" height="auto">
                <template #modal-header>
                    Añadir ítem
                </template>
                
                <template #modal-body>
                    <form class="row">                        
                        <div class="mb-3 col-6">
                            <label for="name" class="form-label">Tipo de ítem</label>
                            <select name="meeting_day" class="input-select">
                                <option value="" disabled selected>Seleccionar</option>
                                <option value="1">Instrumento</option>
                                <option value="2">Otro</option>
                            </select>
                        </div>
                        <div class="mb-3 col-6">
                            <label for="user" class="form-label">Nombre</label>
                            <input type="text" class="input-text" id="user" placeholder="Usuario" required>
                        </div>
                        <div class="mb-3 col-6">
                            <label for="brand" class="form-label">Marca</label>
                            <input type="text" class="input-text" id="brand" placeholder="Marca" required>
                        </div>
                        <div class="mb-3 col-6">
                            <label for="model" class="form-label">Modelo</label>
                            <input type="text" class="input-text" id="model" placeholder="Modelo" required>
                        </div>
                        <div class="mb-3 col-3">
                            <label for="number-serie" class="form-label">Número de serie</label>
                            <input type="number" class="input-text" id="number-serie" placeholder="00000" required>
                        </div>
                        <div class="mb-3 col-3">
                            <label for="dayOfBuy" class="form-label">Fecha de Compra</label>
                            <input type="date" class="input-text" id="dayOfBuy" placeholder="00/00/00" required>
                        </div>
                        <div class="mb-3 col-3">
                            <label for="price" class="form-label">Valor</label>
                            <input type="number" class="input-text" id="price" placeholder="0000" required>
                        </div>
                        <div class="mb-3 col-3">
                            <label for="quantity" class="form-label">Cantidad</label>
                            <input type="number" class="input-text" id="quantity" placeholder="1" required>
                        </div>
                        <div class="d-flex gap-6 container-header mt-4">
                            <ButtonOutline>Añadir Imagen</ButtonOutline>                        
                            <ButtonGeneric>Añadir</ButtonGeneric>
                        </div>        
                    </form>
                </template>
            </Modal>
</template>
