<script setup lang="ts">
/**
 * Componente que muestra información detallada de un país.
 *
 * Props:
 * - country: Objeto con la información del país (nombre, capital, bandera, etc.).
 */

import { defineProps } from "vue";
import type { Country } from "../models/country.model";

// Tipado de props
interface Props {
    country: Country;
}

defineProps<Props>();
</script>

<template>
    <div>
        <!-- Imagen de la bandera del país -->
        <div class="h-56 sm:h-36 overflow-hidden rounded-t-2xl">
            <img class="h-full w-full object-cover" :src="country.flags.png"
                :alt="country.flags.alt || `Flag of ${country.name.common}`" />
        </div>

        <!-- Contenido textual -->
        <div class="p-4 space-y-1 bg-white">
            <h4 class="text-lg font-semibold text-gray-900">
                {{ country.name.common }}
            </h4>

            <p class="text-sm text-gray-600">
                <span class="font-medium text-gray-900">Capital:</span>
                <span v-if="country.capital?.length" class="font-semibold">
                    {{ country.capital.join(', ') }}
                </span>
                <span v-else class="italic text-gray-400">N/A</span>
            </p>

            <p class="text-sm text-gray-600">
                <span class="font-medium text-gray-900">Population:</span>
                <span class="font-semibold">
                    {{ country.population?.toLocaleString() || 'N/A' }}
                </span>
            </p>

            <p class="text-sm text-gray-600">
                <span class="font-medium text-gray-900">Region:</span>
                <span class="font-semibold">
                    {{ country.region || 'N/A' }}
                </span>
            </p>
        </div>
    </div>
</template>
