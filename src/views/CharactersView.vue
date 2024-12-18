<template>
    <div class="container mt-4">
        <h2 class="mb-4" style="color: #aaaaaa;">Character List</h2>

        <!-- Campo de búsqueda -->
        <div class="col-lg-10 mb-3">
            <input 
                v-model="searchQuery" 
                type="text" 
                placeholder="Search characters..." 
                class="form-control"
                style="background-color:#27140e;color: #aaaaaa; border: none;"/>
        </div>

        <div class="col-lg-10">
            <div class="card-container" style="height: 20%; overflow-y: auto">
                <div class="card">
                    <div class="card-body">
                        <!-- Usamos filteredCharacters para filtrar los resultados -->
                        <listItems endpoint="character" :items="filteredCharacters" />
                    </div>
                </div>
            </div>
        </div>

        <!-- <div class="row justify-content-center">
            <div class="col-md-8">
                <button @click="previousPage" :disabled="currentPage <= 1" style="background-color:#27140e;color: #aaaaaa;">Previous</button>
                <span style="color: #aaaaaa; margin-left: 10%; margin-right: 10%;">Page {{ currentPage }} of {{ totalPages }}</span>
                <button @click="nextPage" :disabled="currentPage >= totalPages" style="background-color:#27140e;color: #aaaaaa;">Next</button>
            </div>
        </div> -->
    </div>
</template>

<script setup>
import { ref, computed, onBeforeMount } from "vue";
import listItems from "../components/itemListComponent.vue";

// Variables reactivas
let charactersData = ref([]);
let currentPage = ref(1);
let totalPages = ref();
let perPage = ref(10);

// Campo de búsqueda
let searchQuery = ref("");

// Computed para filtrar los datos según la búsqueda
const filteredCharacters = computed(() => {
    if (!searchQuery.value) {
        return charactersData.value; // Si no hay búsqueda, retorna todos los personajes
    }
    return charactersData.value.filter((character) =>
        character.name.toLowerCase().includes(searchQuery.value.toLowerCase())
    );
});

// Función para obtener los datos de los personajes
async function fetchInfo(pageId = 1) {
    let response = await fetch(`https://theofficeapi.dev/api/characters?page=${pageId}`);
    let data = await response.json();
    totalPages.value = data.meta.pageCount;
    data.results.forEach((character) => {
        charactersData.value.push({
            id: character.id,
            name: character.name,
        });
    });
    
    if (data.meta.currentPage < data.meta.pageCount) {

        return fetchInfo(data.meta.nextPage);
    } else {
        console.log("end");
    }
}
async function fetchCharactersEpisodes(pageId) {
    let response = await fetch(
        `https://theofficeapi.dev/api/characters?limit=9&page=${pageId}`
    );
    let data = await response.json();

    charactersData.value = [];
    totalPages.value = data.meta.pageCount;

    data.results.forEach((character) => {
        charactersData.value.push({
            id: character.id,
            name: character.name,
        });
    });
}

// Funciones para la paginación
function previousPage() {
    if (currentPage.value > 1) {
        currentPage.value--;
        fetchCharactersEpisodes(currentPage.value);
    }
}

function nextPage() {
    if (currentPage.value < totalPages.value) {
        currentPage.value++;
        fetchCharactersEpisodes(currentPage.value);
    }
}

// Montaje inicial
onBeforeMount(() => {
    //fetchCharactersEpisodes(1);
    fetchInfo();
});
</script>
