<script setup lang="ts">
import { Character } from '../models/character.models';
import { defineProps } from 'vue';

interface Props {
    character : Character
};

defineProps<Props>();
</script>

<template>
    <div class="text-center">
        <h4 class="text-xl font-bold text-gray-500">{{ character.name }}</h4>
    </div>
    <div class="h-56 sm:h-36">
        <img :src="character.image" class="h-56 sm:h-36 mx-auto">
    </div>
    <div class="flex items-center justify-center p-4">
        <p class="font-light underline">{{ character.species }}</p>
        <span>|</span>
        <p>{{ character.status }}</p>
    </div>
</template>