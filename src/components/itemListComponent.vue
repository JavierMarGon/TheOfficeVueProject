<template>
    <b-container fluid class="mb-3">
        <b-row v-if="items.length > 0" class="g-3">
            <b-col
                    v-for="item in items"
                    :key="item.id"
                    cols="12"
                    sm="6"
                    md="6"
                    lg="3"
                    xl="3"
                    class="mb-3"
            >
                <RouterLink :to="{ name: endpoint, params: { id: item.id } }" class="btn  btn-lg me-2 mb-2" style="background-color:#27140e;">
                    
                    <p style="color:#aaaaaa">{{ item.name }}</p>
                    
                </RouterLink>
            </b-col>
        </b-row>
        <p v-else>Loading...</p>
    </b-container>
</template>


<script setup>
    import { RouterLink } from 'vue-router';
    
    defineProps({
        items: {
            type: Array,
            default: () => []
        },
        endpoint: {
            type: String,
            required: true
        }
    });
</script>