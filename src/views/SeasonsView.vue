<template>
    <listItems endpoint="season" :items="seasonsData"/>
</template>
<script setup>
    import{ref,onBeforeMount,onMounted} from 'vue';
    import listItems from '../components/itemListComponent.vue'; 
    let seasonsData = ref([]);
    async function fetchData() {
        let response = await fetch("https://theofficeapi.dev/api/seasons");
        let data = await response.json();
        seasonsData.value=[];
        data.forEach((season) => {
            seasonsData.value.push({
                id: season.id,
                name: "season "+season.number
            });
        });
    }
    onBeforeMount ( async() => {
        fetchData();
        console.log(seasonsData);
    });
</script>