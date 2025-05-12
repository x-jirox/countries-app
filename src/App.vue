<script setup lang="ts">
/**
 * Componente principal que maneja la carga, búsqueda, paginación
 * y renderizado de la lista de países.
 */

import { onMounted, ref, watch } from "vue";
import PageHeader from "./components/HeaderPage.vue";
import CountryList from "./components/CountryList.vue";
import axiosClient from "./utils/axios";
import type { Country } from "./models/country.model";

// Estados reactivos
const countries = ref<Country[]>([]);
const search = ref("");
const filteredCountries = ref<Country[]>([]);
const page = ref(1);
const itemsPerPage = ref(12);
const paginatedCountries = ref<Country[]>([]);
const totalItems = ref(0);

// Fetch inicial de países
const fetchCountries = async () => {
    try {
        const { data } = await axiosClient.get("/all");
        countries.value = data;
        totalItems.value = data.length;
    } catch (error) {
        console.error("Error al obtener países:", error);
    }
};

// Filtro por búsqueda
const filterCountries = () => {
    filteredCountries.value = countries.value.filter((country) =>
        country.name.common.toLowerCase().includes(search.value.toLowerCase())
    );
    page.value = 1; // Reset page al buscar
};

// Paginación
const sliceCountries = (currentCountries: Country[]) => {
    const start = (page.value - 1) * itemsPerPage.value;
    const end = page.value * itemsPerPage.value;
    paginatedCountries.value = currentCountries.slice(start, end);
};

// Cambio de página
const changePage = (newPage: number) => {
    page.value = newPage;
};

// Fetch inicial
onMounted(fetchCountries);

// Actualiza vista al cambiar página, datos o búsqueda
watch([countries, page, filteredCountries], () => {
    const source =
        filteredCountries.value.length === 0 && search.value === ""
            ? countries.value
            : filteredCountries.value;
    sliceCountries(source);
});
</script>

<template>
    <div class="min-h-screen ">
        <PageHeader />

        <div class="container max-w-screen-lg mx-auto px-6 py-8">
            <!-- Input de búsqueda -->
            <div class="mb-8">
                <input v-model="search" @input="filterCountries" type="text" placeholder="Search by country name"
                    class="w-full border border-gray-900 rounded-lg p-3 px-4 text-sm shadow-sm focus:outline-none focus:ring-2 focus:ring-black transition" />
            </div>

            <!-- Controles de paginación -->
            <div class="mb-8 flex justify-center space-x-4">
                <button :disabled="page <= 1" @click="changePage(page - 1)"
                    class="px-4 py-2 text-sm font-medium bg-white border border-gray-300 rounded-md hover:bg-gray-100 transition disabled:opacity-50 disabled:cursor-not-allowed">
                    Previous
                </button>
                <button :disabled="page >= totalItems / itemsPerPage" @click="changePage(page + 1)"
                    class="px-4 py-2 text-sm font-medium bg-white border border-gray-300 rounded-md hover:bg-gray-100 transition disabled:opacity-50 disabled:cursor-not-allowed">
                    Next
                </button>
            </div>

            <!-- Lista de países -->
            <CountryList :countries="paginatedCountries" />
        </div>
    </div>
</template>

<style scoped>
/* Puedes extender estilos globales aquí si es necesario */
</style>
