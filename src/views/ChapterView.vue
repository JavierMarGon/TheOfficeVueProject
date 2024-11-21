<template>
    <div id="chapter-view">
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
    let mainCast=ref([]);
    let subCast=ref([]);

    async function fetchCharacters(pageId=1) {
        let response = await fetch("https://theofficeapi.dev/api/episodes?includeCharacters=true&page="+pageId);
        let data = await response.json();
        data.results.forEach((chapter) => {
            console.log(chapter.id+" == "+idChapter);
            if(chapter.id==idChapter){
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
        console.log(data.meta.currentPage+" == "+data.meta.pageCount);
        if(data.meta.currentPage<data.meta.pageCount){
            
            return fetchCharacters(data.meta.nextPage);
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
        fetchCharacters();
        
    });


</script>