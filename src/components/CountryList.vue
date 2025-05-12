<script setup lang="ts">
/**
 * Este componente renderiza una lista de países usando un diseño responsive.
 * Cada país es representado por un componente `CountryItem`.
 * 
 * Props:
 * - countries: Lista de objetos tipo Country.
 */

import { defineProps } from "vue";
import type { Country } from "../models/country.model";
import CountryItem from "../components/CountryItem.vue";

interface Props {
    countries: Country[];
}

defineProps<Props>();
</script>

<template>
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6 p-4">
        <div v-for="country in countries" 
            class="bg-white shadow-md rounded-t-2xl overflow-hidden transition-transform hover:scale-105 hover:shadow-xl">
            <CountryItem :country="country" />
        </div>
    </div>
</template>
