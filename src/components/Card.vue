<script setup lang="ts">
import { PhCheckCircle, PhHeart, PhPlusCircle } from '@phosphor-icons/vue';
// import { inject } from 'vue';

const props = defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number,
  isFavourite: Boolean,
  isAdded: Boolean,
  onClickAdd: Function,
  onClickFavorite: Function
});

// const addToFavorite = inject('addToFavorite');
//
// const onClickFavorite = () => {
//   const obj = {
//     ...props,
//     parentId: props.id
//   };
//
//   addToFavorite(obj);
// };
</script>

<template>
  <div
    class="relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer transition hover:-translate-y-2 hover:shadow-xl"
  >
    <PhHeart
      v-if="onClickFavorite"
      :size="22"
      class="absolute top-8 left-8"
      :weight="isFavourite ? 'fill' : 'regular'"
      :color="isFavourite ? '#c81919' : undefined"
      @click="onClickFavorite"
    />
    <img
      :src="
        imageUrl
          ? 'https://wegotthiscovered.com/wp-content/uploads/2023/11/8763399-1-1.jpg'
          : 'https://wegotthiscovered.com/wp-content/uploads/2023/11/8763399-1-1.jpg'
      "
      alt="123"
    />
    <p class="mt-2">{{ title }}</p>
    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400">price:</span>
        <b>{{ price }} c</b>
      </div>
      <div v-if="onClickAdd">
        <PhCheckCircle v-if="isAdded" :size="22" @click="onClickAdd" />
        <PhPlusCircle v-else :size="22" @click="onClickAdd" />
      </div>
    </div>
  </div>
</template>
