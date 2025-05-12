<script setup lang="ts">
import { defineProps } from 'vue';
import { Country } from '../models/country.model';
import { CountryItem } from './CountryItem.vue';

interface Props {
    countries: Country[];
}

defineProps < Props > ();
</script>

<template>
    <div class="grid grid-cols-1 gap-4 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4">
        <div v-for="country in countries" class="flex items-center justify-center">
            <CountryItem :country="country" />
        </div>
    </div>
</template>