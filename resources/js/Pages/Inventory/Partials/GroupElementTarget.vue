<script setup>
import { ref, onMounted, computed } from "vue";
const isActiveCollapse = ref(false);

import { permissions } from "@/utils/inertiaUtils";
import ButtonGeneric from "@/Components/ButtonGeneric.vue";
import ButtonOutline from "@/Components/ButtonOutline.vue";

const isHovered = ref(false);
const isAnimating = ref(false);

const handleBootstrapEvent = (event) => {
    event.stopPropagation();
};
const props = defineProps({
    obj: Object,
});

onMounted(() => {
    const collapseTriggerElement = document.querySelector(
        ".container-target.container-rounded"
    );

    collapseTriggerElement.addEventListener(
        "click",
        handleBootstrapEvent,
        true
    );

    const collapseElement = document.getElementById("collapse" + props.obj.id);

    collapseElement.addEventListener("shown.bs.collapse", () => {
        isAnimating.value = false;
        isActiveCollapse.value = true;
    });

    collapseElement.addEventListener("hidden.bs.collapse", () => {
        isAnimating.value = false;
        isActiveCollapse.value = false;
    });
});

const getColor = (status) => {
    if (status === "Disponible") return "#24AB21";
    if (status === "Prestamo") return "#F59D16";
    if (status === "Reparación") return "#DA0505";
    if (status === "No disponible") return "#202020";
};
</script>

<template>
    <div
        class="container-fluid relative-container"
        @mouseenter="isHovered = true"
        @mouseleave="isHovered = false"
    >
        <div
            class="container-target container-rounded d-flex justify-content-between mt-3"
            @click="isActiveCollapse = !isActiveCollapse"
            :class="{ isActiveCollapse }"
            type="button"
            data-bs-toggle="collapse"
            :data-bs-target="'#collapse' + props.obj.id"
            :aria-expanded="isActiveCollapse"
            :aria-controls="props.obj.id"
            :style="
                !isActiveCollapse
                    ? 'border-bottom: 4px solid' + getColor(props.obj.status)
                    : ''
            "
        >
            <div class="d-flex justify-content-between w-full">
                <div class="d-flex items-center justify-content-around">
                    <div class="d-flex items-center">
                        <i
                            class="fa-solid fa-music mr-2"
                            style="color: #3c7ff8"
                        ></i>
                        <div class="mr-20 font-semibold">
                            {{ props.obj.type }}
                        </div>
                    </div>
                    <div
                        class="mr-20 text-lg font-semibold"
                        style="color: #3c7ff8"
                    >
                        {{ props.obj.name }}
                    </div>
                    <div class="mr-2">{{ props.obj.brand }}</div>
                </div>
                <div class="d-flex items-center gap-3">
                    <div class="font-semibold">{{ props.obj.status }}</div>
                    <i
                        v-if="props.obj.status === 'Disponible'"
                        class="fa-solid fa-circle-check"
                        style="color: #24ab21"
                    ></i>
                    <i
                        v-if="props.obj.status === 'Prestamo'"
                        class="fa-solid fa-circle-xmark"
                        style="color: #f59d16"
                    ></i>
                    <i
                        v-if="props.obj.status === 'Reparación'"
                        class="fa-solid fa-circle-info"
                        style="color: #da0505"
                    ></i>
                    <i
                        v-if="props.obj.status === 'No disponible'"
                        class="fa-solid fa-triangle-exclamation"
                        style="color: #202020"
                    ></i>
                </div>
            </div>
        </div>

        <div
            class="container-collapse"
            :style="
                isActiveCollapse
                    ? 'border-bottom: 4px solid' + getColor(props.obj.status)
                    : ''
            "
        >
            <div
                class="collapse p-1"
                :class="'collapse' + props.obj.id"
                :id="'collapse' + props.obj.id"
            >
                <div
                    class="container-header d-flex items-center justify-content-between"
                >
                    <div class="font-semibold">
                        Modelo:<span class="d-block font-normal">{{
                            props.obj.model
                        }}</span>
                    </div>
                    <div class="font-semibold">
                        Num de serie:<span class="d-block font-normal">{{
                            props.obj.serial_number
                        }}</span>
                    </div>
                    <div class="font-semibold">
                        Fecha de compra:<span class="d-block font-normal">{{
                            props.obj.date_of_acquisition
                        }}</span>
                    </div>
                    <div class="font-semibold">
                        Valor:<span class="d-block font-normal">{{
                            props.obj.value
                        }}</span>
                    </div>
                    <div class="font-semibold">X {{ props.obj.quantity }}</div>
                </div>
                <hr class="bg-black rounded-xl" />
                <div class="container-header">
                    <div class="font-semibold">
                        Observaciones:
                        <table class="table-auto w-full mt-2 mb-4">
                            <thead>
                                <tr>
                                    <th>Nombre de usuario</th>
                                    <th>Apellido</th>
                                    <th>Fecha Real</th>
                                    <th>Fecha</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr
                                    v-for="item in props.obj.observations"
                                    :key="item.user_id"
                                >
                                    <td>{{ item.user_name }}</td>
                                    <td>{{ item.user_surname }}</td>
                                    <td>{{ item.real_return_date }}</td>
                                    <td>{{ item.return_date }}</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                    <div
                        style="color: #3c7ff8"
                        className="text-center cursor-pointer underline underline-offset-8"
                    >
                        Ver Imagen
                    </div>
                </div>
                <hr class="bg-black rounded-xl" />
                <div class="d-flex gap-6 container-header">
                    <ButtonOutline
                        >Historial Prestamos
                        <i class="fa-solid fa-plus ml-4"></i
                    ></ButtonOutline>
                    <ButtonOutline
                        >Historial Reparaciones
                        <i class="fa-solid fa-plus ml-4"></i
                    ></ButtonOutline>
                    <ButtonGeneric
                        >Realizar Prestamo <i class="fa-solid fa-plus ml-4"></i
                    ></ButtonGeneric>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.collapse,
.collapsing {
    width: 100%;
}

.show {
    visibility: visible;
}

.card-header {
    display: flex;
    flex-direction: row;
    padding: 1rem 7rem;
    align-items: center;
    background-color: #fff;
}

.container-target {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: transparent;
    min-height: 4rem;
    height: auto;
    padding: 0rem 9.5rem 0rem 6.5rem;
    box-shadow: 0px 0px 8px 0px rgba(0, 0, 0, 0.2);
    cursor: pointer;
}

.target-info {
    display: flex;
    flex-direction: column;
    align-items: flex-end;
    padding-right: rem;
}

.container-collapse {
    display: flex;
    border-bottom-right-radius: 3rem;
    border-bottom-left-radius: 3rem;
    box-shadow: 0px 2px 6px 0px rgba(0, 0, 0, 0.2);
}

.isActiveCollapse {
    border-top-left-radius: 3rem;
    border-top-right-radius: 3rem;
    border-bottom-right-radius: 0rem;
    border-bottom-left-radius: 0rem;
    margin: 0rem 0rem;
}

.container-header .fa-circle-plus {
    margin-left: 2rem;
    font-size: 2.5rem;
    color: #2b5db6;
    cursor: pointer;
}

.container-header {
    width: 100%;
    margin: auto;
    padding: 1.5rem 9.5rem 1.5rem 6.5rem;
}

.container-body {
    border-bottom-right-radius: 6rem;
    border-bottom-left-radius: 6rem;
    padding-top: 0rem;
}
</style>
