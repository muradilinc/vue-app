<script setup lang="ts">
import { PhMagnifyingGlass } from '@phosphor-icons/vue';
import CardList from '@/components/CardList.vue';
import axios from 'axios';
import { inject, onMounted, reactive, ref, watch } from 'vue';
import type { Item, Params } from '@/App.vue';
const { cart, addToCart, removeFromCart } = inject('cart');
import debounce from 'lodash.debounce';

const items = ref<Item[]>([]);

const filter = reactive({
  sortBy: 'title',
  searchQuery: ''
});

const onClickAddPlus = (item: Item) => {
  if (!item.isAdded) {
    addToCart(item);
  } else {
    removeFromCart(item);
  }
};

const onChangeSelect = (event: Event) => {
  const select = event.target as HTMLSelectElement;
  filter.sortBy = select.value;
};

const onChangeSearchInput = debounce((event: Event) => {
  const input = event.target as HTMLInputElement;
  filter.searchQuery = input.value;
}, 300);

const getItems = async () => {
  const params: Params = {
    sortBy: filter.sortBy
  };

  if (filter.searchQuery) {
    params.title = `*${filter.searchQuery}*`;
  }

  try {
    const { data } = await axios.get(`https://caa092814b11f7a5.mokky.dev/items`, { params });
    items.value = data.map((obj: Item) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }));
  } catch (error) {
    console.log(error);
  }
};

const getFavourites = async () => {
  try {
    const { data: favorites } = await axios.get<Item[]>(
      `https://caa092814b11f7a5.mokky.dev/favorites`
    );
    items.value = items.value.map((item: Item) => {
      const favorite = favorites.find((favorite) => favorite.item_id === item.id);

      if (!favorite) {
        return item;
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      };
    });
  } catch (error) {
    console.log(error);
  }
};

const addToFavorite = async (item: Item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        item_id: item.id
      };

      const { data } = await axios.post('https://caa092814b11f7a5.mokky.dev/favorites', obj);
      item.isFavorite = true;
      item.favoriteId = data.id;
    } else {
      await axios.delete(`https://caa092814b11f7a5.mokky.dev/favorites/${item.favoriteId}`);
      item.isFavorite = false;
      item.favoriteId = null;
    }
  } catch (error) {
    console.log(error);
  }
};

onMounted(
  // fetch('https://604781a0efa572c1.mokky.dev/items')
  //   .then((res) => res.json())
  //   .then((data) => {
  //     console.log(data);
  //   });
  async () => {
    const localCart = localStorage.getItem('cart');
    cart.value = localCart ? JSON.parse(localCart) : [];

    await getItems();
    await getFavourites();

    items.value = items.value.map((item) => ({
      ...item,
      isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
    }));
  }
);

watch(filter, getItems); // watch(() => filter.sortBy, () =>void)
watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }));
});
</script>

<template>
  <div class="flex justify-between items-center">
    <h2 class="text-3xl font-bold mb-8">All sneakers</h2>
    <div class="flex gap-4">
      <select
        name="filter"
        class="py-2 px-3 border rounded-md outline-none"
        @change="onChangeSelect"
      >
        <option value="name">name</option>
        <option value="price">price (cheap)</option>
        <option value="-price">price (expensive)</option>
      </select>
      <div class="relative">
        <PhMagnifyingGlass :size="22" class="absolute left-4 top-3" />
        <input
          type="text"
          placeholder="Search..."
          @input="onChangeSearchInput"
          class="border border-gray-200 rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
        />
      </div>
    </div>
  </div>
  <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
</template>
