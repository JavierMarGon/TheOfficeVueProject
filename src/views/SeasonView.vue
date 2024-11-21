<template>

    <listItems endpoint="chapter" :items="chaptersData"/>
    <div class="pagination-container">
        <button @click="previousPage" :disabled="currentPage <= 1">Previous</button>
        <span>Page {{ currentPage }} of {{ totalPages }}</span>
        <button @click="nextPage" :disabled="currentPage >= totalPages">Next</button>
    </div>
</template>
<script setup>
    import{ref,onBeforeMount,onMounted} from 'vue';
    import listItems from '../components/itemListComponent.vue'; 
    import { useRoute } from 'vue-router';
    const route = useRoute();
    const idSeason = route.params.id;
    let seasonsData = ref([]);
    let chaptersData = ref([]);
    let currentPage = ref(1);
    let totalPages=ref();
    async function fetchSeasonData() {
        let response = await fetch("https://theofficeapi.dev/api/seasons");
        let data = await response.json();
        seasonsData.value=[];
        data.forEach((season) => {
            if (season.id==idSeason) {
                seasonsData.value=season;
            }
        });
    }
    async function fetchChapterDataBySeason(pageId=1) {
        let response = await fetch("https://theofficeapi.dev/api/episodes?limit=6&season="+idSeason+"&page="+pageId);
        let data = await response.json();
        chaptersData.value=[];
        totalPages.value=data.meta.pageCount;
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
    onBeforeMount ( async() => {
        fetchSeasonData();
        fetchChapterDataBySeason();
        console.log(seasonsData);
    });
</script>