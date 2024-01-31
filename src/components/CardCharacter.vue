<script setup lang="ts">
// Importar la función defineProps de Vue para definir las propiedades del componente
import { defineProps } from 'vue';

// Importar la interfaz Character desde el archivo correcto
import { Character } from '../models/character.models.ts';

// Importar el componente CardItem
import CardItem from './CardItem.vue';

// Definir la interfaz de las propiedades esperadas por el componente
interface Props {
    characters: Character[];
}

// Usar la función defineProps para declarar las propiedades y sus tipos
defineProps<Props>();

</script>

<template>
    <!-- Contenedor de cuadrícula con columnas responsivas -->
    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-3">
        <!-- Iterar sobre la lista de personajes y renderizar el componente CardItem para cada uno -->
        <div class="border border-gray-300 rounded p-3" v-for="character in characters" :key="character.id">
            <!-- Pasar el personaje como propiedad al componente CardItem -->
            <CardItem :character="character"></CardItem>
        </div>
    </div>
</template>
