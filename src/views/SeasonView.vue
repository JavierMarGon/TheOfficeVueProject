<template>
    <div class="container mt-4">
        <h2 class="mb-4" style="color: #aaaaaa;">Season {{ seasonsData.number }}</h2>
        <!-- Use v-for directly on charactersData -->
        <div class="col-lg-10">
            <div class="card-container" style="height: 20%; overflow-y: auto;">
                <div class="card">
                    <div class="card-body">
                        <div class="row">
                            <div class="col-md-6 mb-3">
                                <p>Season Release Date:</p>
                                <p>{{ seasonsData.startDate }}</p>
                            </div>
                            <div class="col-md-6 mb-3">
                                <p>End Season:</p>
                                <p>{{ seasonsData.endDate }}</p>
                            </div>
                        </div>
                        <listItems endpoint="chapter" :items="chaptersData" />
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
import { useRoute } from 'vue-router';
const route = useRoute();
const idSeason = route.params.id;
let seasonsData = ref([]);
let chaptersData = ref([]);
let currentPage = ref(1);
let totalPages = ref();
async function fetchSeasonData() {
    let response = await fetch("https://theofficeapi.dev/api/seasons");
    let data = await response.json();
    seasonsData.value = [];
    data.forEach((season) => {
        if (season.id == idSeason) {
            seasonsData.value = season;
        }
    });
}
async function fetchChapterDataBySeason(pageId = 1) {
    let response = await fetch("https://theofficeapi.dev/api/episodes?limit=6&season=" + idSeason + "&page=" + pageId);
    let data = await response.json();
    chaptersData.value = [];
    totalPages.value = data.meta.pageCount;
    data.results.forEach((chapter) => {
        chaptersData.value.push({
            id: chapter.id,
            name: chapter.title
        });
    });
    // if(data.meta.currentPage<data.meta.pageCount){
    //     return fetchChapterDataBySeason(data.meta.nextPage);
    // }else{
    //     console.log("end");
    // }
}
function previousPage() {
    if (currentPage.value > 1) {
        currentPage.value--;
        fetchChapterDataBySeason(currentPage.value);
    }
}

// Function to handle next page navigation
function nextPage() {
    if (currentPage.value < totalPages.value) {
        currentPage.value++;
        fetchChapterDataBySeason(currentPage.value);
    }
}
onBeforeMount(async () => {
    fetchSeasonData();
    fetchChapterDataBySeason();
    console.log(seasonsData);
});
</script>