<template>
    <div id="character-view">
        <h1 class="character-view-title">{{ characterData.name }}</h1>
        <div class="character-info">
            <div class="character-info-data">
            <div class="character-info-data-item-container">
                <p>Gender: </p>
                <p class="character-info-data-item">{{ characterData.gender }}</p>
            </div>
            <div class="character-info-data-item-container">
                <p>Married: </p>
                <p class="character-info-data-item">{{ characterData.marital }}</p>
            </div>
            <div class="character-info-data-item-container">
                <p>Jobs: </p>
                <div class="item">
                    <div  v-for="(job, index) in characterData.job" :key="index">
                        <p>{{ job }}</p>
                    </div>
                </div>
            </div>
            <div class="character-info-data-item-container">
                <p>Workplaces: </p>
                <div class="item">
                    <div  v-for="(workplace, index) in characterData.workplace" :key="index">
                        <p>{{ workplace }}</p>
                    </div>
                </div>
            </div>
            <div class="character-info-data-item-container">
                <p>First Appearance: </p>
                <p class="character-info-data-item">{{ characterData.firstAppearance }}</p>
            </div>
            <div class="character-info-data-item-container">
                <p>Last Appearance: </p>
                <p class="character-info-data-item">{{ characterData.lastAppearance }}</p>
            </div>
            <div class="character-info-data-item-container">
                <p>Actor: </p>
                <p class="character-info-data-item">{{ characterData.actor }}</p>
            </div>
            </div>
        </div>
        <h1 class="character-episode-header">List of episodes with {{ characterData.name }}</h1>
        <listItems endpoint="chapter" :items="episodesData"/>
    </div>
</template>
<script setup>
    import{ref,onBeforeMount,onMounted} from 'vue';
    import listItems from '../components/itemListComponent.vue'; 
    import { useRoute } from 'vue-router';
    const route = useRoute();
    const idCharacter = route.params.id;
    let characterData= ref([]);
    let episodesData=ref([]);
    async function fetchData() {
        let response = await fetch("https://theofficeapi.dev/api/character/"+idCharacter);
        let data = await response.json();
        characterData.value=data;
        
    }
    async function fetchEpisodeData() {
        let response = await fetch("https://theofficeapi.dev/api/character/"+idCharacter+"?includeEpisodes=true");
        let data = await response.json();
        data.episodes.forEach((ep) => {
            episodesData.value.push({
                id: ep.episode.id,
                name: ep.episode.title
            });
        });
    }
    onBeforeMount ( async() => {
        fetchData();
    });
    onMounted(async () => {
        fetchEpisodeData();
    })
</script>