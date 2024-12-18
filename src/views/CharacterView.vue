<template>
  <div class="container mt-5">
    <h1 class="mb-4" style="color: #aaaaaa;">{{ characterData.name }}</h1>

    <div class="row">
      <div class="col-md-8 mb-4">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">Character Information</h5>

            <div class="row">
              <div class="col-md-6 mb-3">
                <div class="card h-100">
                  <img :src="getImageUrl(characterImage)" :alt="characterData.name" class="card-img-top img-fluid"
                    style="width: 100%; height: auto;">
                </div>
              </div>
              <div class="col-md-6 mb-3">
                <p>Gender:</p>
                <p class="card-text">{{ characterData.gender }}</p>
              
                <p>Married:</p>
                <p class="card-text">{{ characterData.marital }}</p>

                <p>First Appearance:</p>
                <p class="card-text">{{ characterData.firstAppearance }}</p>

                <p>Last Appearance:</p>
                <p class="card-text">{{ characterData.lastAppearance }}</p>

                <p>Actor:</p>
                <p class="card-text">{{ characterData.actor }}</p>
              </div>
            </div>

            <div class="mb-3">
              <h6 class="card-subtitle">Jobs:</h6>
              <ul class="list-group">
                <li v-for="(job, index) in characterData.job" :key="index" class="list-group-item">
                  {{ job }}
                </li>
              </ul>
            </div>

            <div class="mb-3">
              <h6 class="card-subtitle">Workplaces:</h6>
              <ul class="list-group">
                <li v-for="(workplace, index) in characterData.workplace" :key="index" class="list-group-item">
                  {{ workplace }}
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>

      <div class="col-md-4">
        <div class="card-container" style="height: 550px; overflow-y: auto;">
          <div class="card">
            <div class="card-body">
              <h5 class="card-title">Episodes</h5>
              <listItems endpoint="chapter" :items="episodesData" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref, onBeforeMount, onMounted } from 'vue';
import listItems from '../components/itemListComponent.vue';
import { useRoute } from 'vue-router';
import imgSource from '../assets/imgSourceIndex.json';
const route = useRoute();
const idCharacter = route.params.id;
let characterData = ref([]);
let characterImage;
let episodesData = ref([]);

function fetchImg(id) {
  imgSource.results.forEach((character) => {
    if (character.id == id) {
      characterImage = character.img;
    }
  });
}
async function fetchData() {
  let response = await fetch("https://theofficeapi.dev/api/character/" + idCharacter);
  let data = await response.json();
  characterData.value = data;

}
async function fetchEpisodeData() {
  let response = await fetch("https://theofficeapi.dev/api/character/" + idCharacter + "?includeEpisodes=true");
  let data = await response.json();
  data.episodes.forEach((ep) => {
    episodesData.value.push({
      id: ep.episode.id,
      name: ep.episode.title
    });
  });
}
const getImageUrl = (imageName) => {
  return new URL(`../assets/images/${imageName}`, import.meta.url).href;
};

onBeforeMount(async () => {
  fetchData();
});
onMounted(async () => {
  fetchEpisodeData();
  fetchImg(idCharacter);
  console.log(characterImage);
})
</script>