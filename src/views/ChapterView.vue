<template>
    <div id="chapter-view">
        <div class="chapter-info">
            <div class="chapter-info-container">
                <div class="chapter-info-item">
                    <p>Episode Name: </p>
                    <p>{{ chapterInfo.title }}</p>
                </div>
                <div class="chapter-info-item">
                    <p>{{ chapterInfo.summary }}</p>
                </div>
                <div class="chapter-info-item">
                    <p>Episode Number:</p>
                    <p>{{ chapterInfo.episode }}</p>
                </div>
                <div class="chapter-info-item">
                    <p>Episode Air Date:</p>
                    <p>{{ chapterInfo.airDate }}</p>
                </div>
                <div class="chapter-info-item">
                    <p>Episode Season:</p>
                    <p>{{ chapterInfo.seasonId }}</p>
                </div>
            </div>
        </div>
        <charactersSlider title="Main Cast" :characters="mainCast"/>
        <charactersSlider title="Sub Cast" :characters="subCast"/>
    </div>
    
</template>
<script setup>
    import {ref,onBeforeMount} from 'vue';
    import { useRoute } from 'vue-router';
    import charactersSlider from '../components/sliderCarrouselComponent.vue';
    const route = useRoute();
    const idChapter = route.params.id;
    let chapterInfo=ref([]);
    let mainCast=ref([]);
    let subCast=ref([]);

    async function fetchInfo(pageId=1) {
        let response = await fetch("https://theofficeapi.dev/api/episodes?includeCharacters=true&page="+pageId);
        let data = await response.json();
        data.results.forEach((chapter) => {
            
            if(chapter.id==idChapter){
                chapterInfo.value=chapter;
                chapter.mainCharacters.forEach((main) => {
                    mainCast.value.push({
                        id: main.id,
                        name: main.name
                    });
                });
                chapter.recurringCharacters.forEach((sub) => {
                    subCast.value.push({
                        id: sub.id,
                        name: sub.name
                    });
                });
            }
        });
        if(data.meta.currentPage<data.meta.pageCount){
            
            return fetchInfo(data.meta.nextPage);
        }else{
            console.log("end");
        }
    }
    // async function fetchCharactersEpisodes(pageId) {
    //     let response = await fetch("https://theofficeapi.dev/api/characters?page="+pageId+"&includeEpisodes=true");
    //     let data = await response.json();
    //     data.results.forEach((character) => {
    //         character.episodes.forEach((ep) => {
    //             if (ep.episode.id==idChapter) {
    //                 cast.value.push({
    //                     id: character.id,
    //                     name: character.name
    //                 });
    //             }
    //         });
    //     });
    //     if(data.meta.currentPage!=data.meta.pageCount){
    //         return fetchCharactersEpisodes(data.meta.nextPage);
    //     }
    // }
    onBeforeMount ( async() => {
        fetchInfo();
        
    });


</script>