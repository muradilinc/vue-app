<script setup lang="ts">
import CartItem from '@/components/CartItem.vue';
import { inject } from 'vue';
import type { Item } from '@/App.vue';

const { cart, removeFromCart } = inject('cart') as { cart: Item[]; removeFromCart: Function };
</script>

<template>
  <div class="flex flex-col gap-y-5 flex-1 justify-between" v-auto-animate>
    <CartItem
      v-for="item in cart"
      :key="item.id"
      :title="item.title"
      :price="item.price"
      :imageUrl="item.imageUrl"
      @on-click-remove="() => removeFromCart(item)"
    />
  </div>
</template>
