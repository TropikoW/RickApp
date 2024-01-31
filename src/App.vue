<script setup lang="ts">
  import { ref, onMounted , watch} from 'vue';
  import PageHeader from './components/PageHeader.vue';
  import CardCharacter from './components/CardCharacter.vue';
  import AxiosClient from './utils/axios.ts';
  import { Character } from '../src/models/character.models';

  const characters = ref<Character[]>([]);
  const searchUser = ref('');
  const filteredSearch = ref<Character[]>([]);
  const page = ref(1);
  const itemPerPage = ref(6);
  const paginatedCharacter = ref<Character[]>([]);
  const totalItems = ref(0);

  const fetchApi = async () => {
    try {
      const { data } = await AxiosClient.get('/character');
      characters.value = data.results;
      totalItems.value = characters.value.length;
    } catch (error) {
      console.error('Error al obtener datos de la API:', error);
      throw new Error('Something went wrong');
    }
  };

  const filterCharacters = () => {
    filteredSearch.value = characters.value.filter((character) =>
      character.name.toLowerCase().includes(searchUser.value.toLowerCase())
    );
  };

  const sliceCharacters = (currentCharacters : Character[]) => {
    const start = (page.value -1)* itemPerPage.value;
    const end = page.value * itemPerPage.value;
    paginatedCharacter.value = currentCharacters.slice(start,end);
  }

  const changePage = (newPage : number) => {
    page.value = newPage;
  }

  onMounted(() => {
    fetchApi();
    sliceCharacters(characters.value);
  });

  watch([characters,page,filteredSearch],()=>{
    sliceCharacters(filteredSearch.value.length <= 0 && searchUser.value === ""
    ? characters.value
    : filteredSearch.value
    )
  })
</script>

<template>
  <div>
    <!-- Renderizar el encabezado de la página -->
    <PageHeader></PageHeader>

    <!-- Contenedor principal del componente -->
    <div class="container max-w-screen-lg mx-auto">

      <div class="mb-8 mx-auto">
        <input
          type="text"
          class="border border-gray-300 rounded w-full p-1 px-16"
          placeholder="Search character"
          @input="filterCharacters"
          v-model="searchUser"
        />
      </div>

      <div class="mb-8 flex justify-center space-x-24">
        <button 
          class="border border-gray-300 rounded px-2 py-0.5 hover:bg-gray-200" 
          :class="{'opacity-50':page<= 1}"
          @click="changePage(page - 1)"
          :disabled="page <= 1">
          Previous
        </button>
        <button 
          class="border border-gray-300 rounded px-2 py-0.5 hover:bg-gray-200" 
          @click="changePage(page +1)"
          :class="{'opacity-50':page >= totalItems / itemPerPage}"
          :disabled="page >= totalItems / itemPerPage">
          Next
        </button>
      </div>

      <!-- Renderizar el componente CardCharacter y pasar los personajes como propiedad -->
      <CardCharacter :characters="paginatedCharacter"></CardCharacter>
    </div>
  </div>
</template>
