<template>
    <div id="sliderCarrouselComponent">
        <div class="title">
            <h5 >{{ title }} </h5>
        </div>
        <div class="splide-wrapper">
            <Splide id="thumbnail-carousel" :options="{pagination:false, perPage:5,padding:'5%',gap:'1rem' }" aria-label="Thumbnail Carousel">
                <SplideSlide v-for="(character) in characters">
                    <div class="item-container">
                        <RouterLink :to="{ name: endpoint, params: { id: character.id }}"><p>{{ character.name }}</p></RouterLink>
                    </div>
                </SplideSlide>
            </Splide>
        </div>
    </div>
    
</template>

<script setup>
    import { RouterLink} from 'vue-router';
    const endpoint="character";
    defineProps({
        title: String,
        characters: {
            type: Array,
            default: () => []
        }
    })
        

</script>
