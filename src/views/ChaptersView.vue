<template>
    <div id="chapters-view">
        <h1 class="chapters-title">The Office List of Episodes</h1>
        <listItems endpoint="chapter" :items="episodeData"/>
        <div class="pagination-container">
            <button @click="previousPage" :disabled="currentPage <= 1">Previous</button>
            <span>Page {{ currentPage }} of {{ totalPages }}</span>
            <button @click="nextPage" :disabled="currentPage >= totalPages">Next</button>
        </div>
    </div>
</template>

<script setup>
    import{ref,onBeforeMount,onMounted} from 'vue';
    import listItems from '../components/itemListComponent.vue';
    let episodeData=ref([]);
    let currentPage = ref(1);
    let totalPages=ref();


    async function fetchData(page) {
        let response = await fetch("https://theofficeapi.dev/api/episodes?limit=12&page="+page);
        let data = await response.json();
        episodeData.value=[];
        totalPages.value=data.meta.pageCount;
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
    

    onBeforeMount ( async() => {
        fetchData(1);
        console.log(episodeData);
    });
</script>