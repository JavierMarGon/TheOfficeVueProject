<template>
    <div class="container mt-4">
        <h2 class="mb-4" style="color: #aaaaaa;">Season List</h2>
        <!-- Use v-for directly on charactersData -->
        <div class="col-lg-10">
            <div class="card-container" style="height: 20%; overflow-y: auto">
                <div class="card">
                    <div class="card-body">
                        <listItems endpoint="season" :items="seasonsData" />
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script setup>
import { ref, onBeforeMount, onMounted } from 'vue';
import listItems from '../components/itemListComponent.vue';
let seasonsData = ref([]);
async function fetchData() {
    let response = await fetch("https://theofficeapi.dev/api/seasons");
    let data = await response.json();
    seasonsData.value = [];
    data.forEach((season) => {
        seasonsData.value.push({
            id: season.id,
            name: "season " + season.number
        });
    });
}
onBeforeMount(async () => {
    fetchData();
    console.log(seasonsData);
});
</script>