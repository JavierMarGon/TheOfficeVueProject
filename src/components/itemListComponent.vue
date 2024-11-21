<template>
    <div id="itemListComponent" v-if="items.length > 0">
        <div v-for="item in items" :key="item.id">
        <RouterLink :to="{ name: endpoint, params: { id: item.id }}">
            <div class="item">
                <p>{{ item.name }}</p>
            </div>
        </RouterLink>
        </div>
    </div>
    <p v-else>Loading...</p>
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