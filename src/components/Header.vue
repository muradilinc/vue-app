<script setup lang="ts">
import { PhHeart, PhShoppingCart, PhUser } from '@phosphor-icons/vue';
defineProps({
  totalPrice: Number
});
const emit = defineEmits(['openDrawer']);
</script>

<template>
  <header class="flex justify-between border-b border-slate-300 p-8">
    <router-link to="/">
      <div class="flex items-center gap-4">
        <img
          src="https://png.pngtree.com/png-clipart/20200826/original/pngtree-modern-sneaker-shoe-logo-png-image_5491765.jpg"
          alt="logo"
          class="w-16"
        />
        <div>
          <h2 class="text-xl font-bold uppercase">Vue Sneakers</h2>
          <p class="text-slate-400">Store the best sneakers</p>
        </div>
      </div>
    </router-link>
    <ul class="flex cursor-pointer items-center gap-10">
      <li
        @click="() => emit('openDrawer')"
        class="flex items-center gap-3 text-slate-500 hover:text-black"
      >
        <PhShoppingCart :size="24" /><b>{{ totalPrice }} c</b>
      </li>
      <router-link to="/favorites">
        <li class="flex cursor-pointer items-center gap-3 text-slate-500 hover:text-black">
          <PhHeart :size="24" /><span>Favourite</span>
        </li>
      </router-link>
      <li class="flex cursor-pointer items-center gap-3 text-slate-500 hover:text-black">
        <PhUser :size="24" /><span>Profile</span>
      </li>
    </ul>
  </header>
</template>
