<script setup lang="ts">
import Header from './components/Header.vue';
import { computed, provide, ref, watch } from 'vue';
import Drawer from '@/components/Drawer.vue';
import axios from 'axios';

export interface Item {
  id: number;
  parentId?: number;
  imageUrl: string;
  title: string;
  price: number;
  isFavorite?: boolean;
  isAdded?: boolean;
  favoriteId?: number | null;
}

export interface Params {
  sortBy: string;
  title?: string;
}

const cart = ref<Item[]>([]);
const drawerOpen = ref(false);

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0));
const taxPrice = computed(() => Math.round(totalPrice.value * 5) / 100);

const removeFromCart = (item: Item) => {
  cart.value.splice(cart.value.indexOf(item), 1);
  item.isAdded = false;
};

const addToCart = (item: Item) => {
  if (!item.isAdded) {
    cart.value.push(item);
    item.isAdded = true;
  } else {
    removeFromCart(item);
  }
};

watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value));
  },
  { deep: true }
);

const closerDrawer = () => {
  drawerOpen.value = false;
};

const openDrawer = () => {
  drawerOpen.value = true;
};

// provide('addToFavorite', addToFavorite);
provide('cart', { closerDrawer, openDrawer, cart, addToCart, removeFromCart });
</script>

<template>
  <Drawer v-if="drawerOpen" :total-price="totalPrice" :taxPrice="taxPrice" />
  <div class="w-4/5 m-auto bg-white rounded-xl shadow-xl mt-14">
    <Header :total-price="totalPrice" @open-drawer="openDrawer" />
    <div class="p-10">
      <router-view> </router-view>
    </div>
  </div>
</template>
