<script setup lang="ts">
defineProps({
  title: String,
  description: String,
  imageUrl: String
});
</script>

<template>
  <div class="flex flex-col items-center text-center w-72 mx-auto">
    <img class="h-[80px] w-[120px]" :src="imageUrl" alt="empty" />
    <h2 class="mt-4 text-2xl font-medium">{{ title }}</h2>
    <p class="text-gray-400">{{ description }}</p>
  </div>
</template>
