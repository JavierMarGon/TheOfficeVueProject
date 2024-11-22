<template>
    <listItems endpoint="character" :items="charactersData"/>
        <div class="pagination-container">
            <button @click="previousPage" :disabled="currentPage <= 1">Previous</button>
            <span>Page {{ currentPage }} of {{ totalPages }}</span>
            <button @click="nextPage" :disabled="currentPage >= totalPages">Next</button>
        </div>
</template>
<script setup>
    import{ref,onBeforeMount,onMounted} from 'vue';
    import listItems from '../components/itemListComponent.vue';
    let charactersData=ref([]);
    let currentPage = ref(1);
    let totalPages=ref();

    async function fetchCharactersEpisodes(pageId) {
        let response = await fetch("https://theofficeapi.dev/api/characters?limit=9&page="+pageId);
        let data = await response.json();
        charactersData.value=[];
        totalPages.value=data.meta.pageCount;
        data.results.forEach((character) => {
            charactersData.value.push({
                id: character.id,
                name: character.name
            });
        });
    }
    function previousPage() {
        if (currentPage.value > 1) {
            currentPage.value--;
            fetchCharactersEpisodes(currentPage.value);
        }
    }

    // Function to handle next page navigation
    function nextPage() {
        if (currentPage.value < totalPages.value) {
            currentPage.value++;
            fetchCharactersEpisodes(currentPage.value);
        }
    }
    

    onBeforeMount ( async() => {
        fetchCharactersEpisodes(1);
    });
</script>