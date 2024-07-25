<script setup lang="ts">
import Card from '@/components/Card.vue';
// import { inject } from 'vue';

defineProps({
  items: Array,
  isFavorite: Boolean
});

const emit = defineEmits(['addToFavorite', 'addToCart']);
// const addToFavorite = inject('addToFavorite') as Function;
</script>

<template>
  <div class="grid grid-cols-4 gap-5 mt-10" v-auto-animate>
    <Card
      v-for="item in items"
      :key="item.id"
      :image-url="item.imageUrl"
      :title="item.title"
      :price="item.price"
      :on-click-add="isFavorite ? null : () => emit('addToCart', item)"
      :on-click-favorite="isFavorite ? null : () => emit('addToFavorite', item)"
      :isFavourite="item.isFavorite"
      :isAdded="item.isAdded"
    />
  </div>
</template>

<!--      :on-click-favorite="() => addToFavorite(item)"-->
