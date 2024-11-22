<template>

</template>
<script setup>
    import{ref,onBeforeMount,onMounted} from 'vue';
    import listItems from '../components/itemListComponent.vue'; 
    import { useRoute } from 'vue-router';
    const route = useRoute();
    const idCharacter = route.params.id;
    let characterData= ref([]);
    async function fetchData() {
        let response = await fetch("https://theofficeapi.dev/api/character/"+idCharacter+"?includeEpisodes=true");
        let data = await response.json();
        characterData.value=data;
    }
    onBeforeMount ( async() => {
        fetchData();
        console.log(characterData);
    });
</script>