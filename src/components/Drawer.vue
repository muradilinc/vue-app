<script setup lang="ts">
import { PhArrowLeft } from '@phosphor-icons/vue';
import CartListItem from '@/components/CartListItem.vue';
import { computed, inject, ref } from 'vue';
import InfoBlock from '@/components/InfoBlock.vue';
import axios from 'axios';

const props = defineProps({
  totalPrice: Number,
  taxPrice: Number
});

const isCreating = ref(false);
const orderId = ref(null);

const { closerDrawer, cart } = inject('cart') as { closerDrawer: Function };

const emptyCart = computed(() => cart.value.length === 0);
const buttonCartDisabled = computed(() => isCreating.value || emptyCart.value);

const createOrder = async () => {
  try {
    isCreating.value = true;
    const { data } = await axios.post('https://caa092814b11f7a5.mokky.dev/orders', {
      items: cart.value,
      totalPrice: props.totalPrice!.value + props.taxPrice!.value
    });

    cart.value = [];
    isCreating.value = false;
    orderId.value = data.id;
    return data;
  } catch (error) {
    console.log(error);
  }
};
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <div class="flex gap-5 items-center mb-4">
      <PhArrowLeft
        @click="closerDrawer!"
        :size="22"
        class="opacity-30 cursor-pointer hover:opacity-100 transition hover:-translate-x-1"
      />
      <h2 class="text-2xl font-bold">Cart</h2>
    </div>
    <div v-if="!totalPrice || orderId" class="flex h-full items-center">
      <InfoBlock
        v-if="orderId"
        title="Order placed"
        :description="`Your order is #${orderId} will be transferred to courier delivery soon.`"
        image-url="https://www.itadori-shop.com/cdn/shop/articles/Satoru-Hollow-Purple-e1615636661895_1200x1200.jpg?v=1634757049"
      />
      <InfoBlock
        v-if="!totalPrice && !orderId"
        title="Cart is empty"
        description="Add at least one pair of sneakers to place an order."
        image-url="https://image.hurimg.com/i/hurriyet/75/750x422/650d85664e3fe117e8a6b176.jpg"
      />
    </div>

    <div v-else>
      <CartListItem />

      <div class="flex flex-col gap-y-4 my-7">
        <div class="flex gap-2">
          <span class="capitalize">total:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ totalPrice }} c</b>
        </div>
        <div class="flex gap-2">
          <span class="capitalize">tax 5%:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ taxPrice }} c</b>
        </div>
        <button
          @click="createOrder"
          class="bg-lime-500 uppercase w-full rounded-xl py-3 text-white hover:bg-lime-600 transition active:bg-lime-700 disabled:bg-slate-300 cursor-pointer"
          :disabled="buttonCartDisabled"
        >
          place an order
        </button>
      </div>
    </div>
  </div>
</template>
