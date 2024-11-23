<template>
    <div id="sliderCarrouselComponent">
        <div class="title">
            <h5 >{{ title }} </h5>
        </div>
        <div class="splide-wrapper">
            <Splide id="thumbnail-carousel" :options="{pagination:false, perPage:5,padding:'5%',gap:'1rem' }" aria-label="Thumbnail Carousel">
                <SplideSlide v-for="(character) in characters">
                    <div class="img-container">
                        <RouterLink :to="{ name: endpoint, params: { id: character.id }}">
                            <img width="100%" :src="getImageUrl(character.image)" :alt="character.name"/>
                        </RouterLink>
                    </div>
                    <p>{{ character.name }}</p>
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
    const getImageUrl = (imageName) => {
        return new URL(`../assets/images/${imageName}`, import.meta.url).href;
    };

</script>
