<script setup lang="ts">
import axios from 'axios';
import { onMounted, ref } from 'vue';
import CardList from '@/components/CardList.vue';

const favorites = ref([]);

onMounted(async () => {
  try {
    const { data } = await axios.get(
      'https://caa092814b11f7a5.mokky.dev/favorites?_relations=items'
    );
    favorites.value = data.map((obj) => obj.item);
  } catch (error) {
    console.log(error);
  }
});

console.log(favorites.value);
</script>

<template>
  <h2>Favorites</h2>
  <CardList :items="favorites" is-favorite />
</template>
