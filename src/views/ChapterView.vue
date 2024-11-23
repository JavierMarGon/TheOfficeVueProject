<template>

    <div class="container mt-5">
        <h1 class="mb-4" style="color: #aaaaaa;">{{ chapterInfo.title }}</h1>

        <div class="row">
            <div class="col-md-8 mb-4">
                <div class="card">
                    <div class="card-body">
                        <h5 class="card-title">Episode Information</h5>

                        <div class="row">
                            <div class="col-md-6 mb-3">
                                <p class="card-text">{{ chapterInfo.summary }}</p>
                            </div>
                            <div class="col-md-6 mb-3">
                                <p>Episode Air Date:</p>
                                <p>{{ chapterInfo.airDate }}</p>
                            </div>
                            <div class="col-md-6 mb-3">
                                <p>Episode Season:</p>
                                <p>{{ chapterInfo.seasonId }}</p>
                            </div>
                        </div>
                        <charactersSlider title="Main Cast" :characters="mainCast" />
                        <charactersSlider title="Sub Cast" :characters="subCast" />
                    </div>
                </div>
            </div>


        </div>
    </div>


</template>
<script setup>
import { ref, onBeforeMount } from 'vue';
import { useRoute } from 'vue-router';
import charactersSlider from '../components/sliderCarrouselComponent.vue';
import imgSource from '../assets/imgSourceIndex.json';
const route = useRoute();
const idChapter = route.params.id;
let chapterInfo = ref([]);
let mainCast = ref([]);
let subCast = ref([]);

function fetchImg(id){
    let result;
    imgSource.results.forEach((character) => {
        if(character.id==id){
            result=character.img;
        }
    });
    return result;
}

async function fetchInfo(pageId = 1) {
    let response = await fetch("https://theofficeapi.dev/api/episodes?includeCharacters=true&page=" + pageId);
    let data = await response.json();
    data.results.forEach((chapter) => {

        if (chapter.id == idChapter) {
            chapterInfo.value = chapter;
            chapter.mainCharacters.forEach((main) => {
                mainCast.value.push({
                    id: main.id,
                    name: main.name,
                    image:fetchImg(main.id)
                });
            });
            chapter.recurringCharacters.forEach((sub) => {
                subCast.value.push({
                    id: sub.id,
                    name: sub.name,
                    image: fetchImg(sub.id)
                });
            });
        }
    });
    if (data.meta.currentPage < data.meta.pageCount) {

        return fetchInfo(data.meta.nextPage);
    } else {
        console.log("end");
    }
}

onBeforeMount(async () => {
    fetchInfo();
    console.log(mainCast);
});


</script>