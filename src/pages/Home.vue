<script setup>
import { inject, onMounted, reactive, ref, watch } from 'vue';
import axios from 'axios';
import debounce from 'lodash.debounce';
import CardList from '../components/CardList.vue';

const { cart, addToCart, removedFromCart } = inject('cart');

const items = ref([]);
const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
});

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item);
  } else {
    removedFromCart(item);
  }
};

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value;
};

const onChangeInput = debounce((event) => {
  filters.searchQuery = event.target.value;
}, 300);

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const payload = {
        parentId: item.id,
        item
      };

      item.isFavorite = true;
      const { data } = await axios.post(
        `https://65a5296852f07a8b4a3e798f.mockapi.io/favorites`,
        payload
      );

      item.favoriteId = data.id;
    } else {
      await axios.delete(
        `https://65a5296852f07a8b4a3e798f.mockapi.io/favorites/${item.favoriteId}`
      );

      item.isFavorite = false;
      item.favoriteId = null;
    }
  } catch (error) {
    console.log(error);
  }
};

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(
      `https://65a5296852f07a8b4a3e798f.mockapi.io/favorites`
    );

    items.value = items.value.map((item) => {
      const isFavorite = favorites.find((favorite) => favorite.parentId === item.id);

      if (!isFavorite) {
        return item;
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: isFavorite.id
      };
    });
  } catch (error) {
    console.log(error);
  }
};

const fetchItems = async () => {
  try {
    const { data } = await axios.get(`https://65a5296852f07a8b4a3e798f.mockapi.io/sneakers`, {
      params: {
        sortBy: filters.sortBy ?? null,
        search: filters.searchQuery || null
      }
    });

    items.value = data.map((item) => ({
      ...item,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }));
  } catch (error) {
    console.log(error);
  }
};

onMounted(async () => {
  const localCart = localStorage.getItem('cart');
  const existingCart = localCart !== 'undefined' && localCart !== null;
  cart.value = existingCart ? JSON.parse(localCart) : [];

  await fetchItems();
  await fetchFavorites();

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: Boolean(cart.value.find((cartItem) => cartItem.id === item.id))
  }));
});
watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }));
});

watch(filters, fetchItems);
</script>

<template>
  <div class="flex justify-between items-center mb-8">
    <h2 class="text-3xl font-bold">All sneakers</h2>
    <div class="flex gap-4 items-center">
      <select class="py-2 px-3 outline-none rounded-md" @change="onChangeSelect">
        <option value="title">By name</option>
        <option value="price">By price (low to high)</option>
        <option value="-price">By price (high to low)</option>
      </select>
      <div class="relative">
        <img src="/search.svg" alt="Search" class="absolute left-4 top-2.5" />
        <input
          @input="onChangeInput"
          placeholder="Search..."
          class="border transition-all border-gray-200 rounded-md py-1.5 pl-11 pr-4 outline-none focus:border-gray-400"
          type="search"
        />
      </div>
    </div>
  </div>
  <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
</template>
