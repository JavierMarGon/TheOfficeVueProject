<template>
    <div class="container mt-4">
        <h2 class="mb-4"style="color: #aaaaaa;">Character List</h2>
        <!-- Use v-for directly on charactersData -->
        <div class="col-lg-10">
            <div class="card-container" style="height: 20%; overflow-y: auto">
                <div class="card">
                    <div class="card-body">
                        <listItems endpoint="character" :items="charactersData" />
                    </div>
                </div>
            </div>
        </div>
        <div class="row justify-content-center">
            <div class="col-md-8">
                <button @click="previousPage" :disabled="currentPage <= 1" style="background-color:#27140e;color: #aaaaaa;">Previous</button>
                <span style="color: #aaaaaa; margin-left: 10%; margin-right: 10%;">Page {{ currentPage }} of {{ totalPages }}</span>
                <button @click="nextPage" :disabled="currentPage >= totalPages" style="background-color:#27140e;color: #aaaaaa;">Next</button>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, onBeforeMount } from "vue";
import listItems from "../components/itemListComponent.vue";

let charactersData = ref([]);
let currentPage = ref(1);
let totalPages = ref();
let perPage = ref(10);

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

onBeforeMount(() => {
    fetchCharactersEpisodes(1);
});
</script>
