<template>
    <div class="container mt-4">
        <h2 class="mb-4" style="color: #aaaaaa;">The Office List of Episodes</h2>
        <!-- Use v-for directly on charactersData -->
        <div class="col-lg-10">
            <div class="card-container" style="height: 20%; overflow-y: auto;">
                <div class="card">
                    <div class="card-body">
                        <listItems endpoint="chapter" :items="episodeData" />
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
import { ref, onBeforeMount, onMounted } from 'vue';
import listItems from '../components/itemListComponent.vue';
let episodeData = ref([]);
let currentPage = ref(1);
let totalPages = ref();


async function fetchData(page) {
    let response = await fetch("https://theofficeapi.dev/api/episodes?limit=12&page=" + page);
    let data = await response.json();
    episodeData.value = [];
    totalPages.value = data.meta.pageCount;
    console.log(totalPages);
    data.results.forEach((episode) => {
        episodeData.value.push({
            id: episode.id,
            name: episode.title
        });
    });
}
function previousPage() {
    if (currentPage.value > 1) {
        currentPage.value--;
        fetchData(currentPage.value);
    }
}

// Function to handle next page navigation
function nextPage() {
    if (currentPage.value < totalPages.value) {
        currentPage.value++;
        fetchData(currentPage.value);
    }
}


onBeforeMount(async () => {
    fetchData(1);
    console.log(episodeData);
});
</script>