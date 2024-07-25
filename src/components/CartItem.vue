<script setup lang="ts">
import { PhXCircle } from '@phosphor-icons/vue';

defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean
});

const emit = defineEmits(['onClickRemove']);
</script>

<template>
  <div class="flex border border-slate-200 p-4 rounded-xl gap-4">
    <img
      class="w-16 h-16"
      :src="
        imageUrl
          ? 'https://wegotthiscovered.com/wp-content/uploads/2023/11/8763399-1-1.jpg'
          : 'https://wegotthiscovered.com/wp-content/uploads/2023/11/8763399-1-1.jpg'
      "
      alt="123"
    />
    <div class="flex flex-col flex-1">
      <p>{{ title }}</p>
      <div class="flex justify-between mt-2">
        <b class="flex-1">{{ price }} c</b>
        <PhXCircle
          @click="emit('onClickRemove')"
          :size="22"
          class="opacity-40 hover:opacity-100 cursor-pointer transition"
        />
      </div>
    </div>
  </div>
</template>
