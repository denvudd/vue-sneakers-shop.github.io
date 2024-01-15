<script setup>
import { computed, provide, ref, watch } from 'vue';
import Header from './components/Header.vue';
import Drawer from './components/Drawer/Drawer.vue';

const cart = ref([]);
const drawerOpen = ref(false);
const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0));
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100));

const closeDrawer = () => {
  drawerOpen.value = false;
};

const openDrawer = () => {
  drawerOpen.value = true;
};

const addToCart = (item) => {
  cart.value.push(item);
  item.isAdded = true;
};

const removedFromCart = (item) => {
  const index = cart.value.findIndex((cartItem) => cartItem.id === item.id);
  cart.value.splice(index, 1);
  item.isAdded = false;
};

watch(cart, () => localStorage.setItem('cart', JSON.stringify(cart.value)), {
  deep: true
});
watch(drawerOpen, () => {
  if (drawerOpen.value) {
    document.body.style.overflow = 'hidden';
  } else {
    document.body.style.overflow = 'unset';
  }
});
provide('cart', {
  cart,
  closeDrawer,
  openDrawer,
  addToCart,
  removedFromCart
});
</script>

<template>
  <Drawer v-if="drawerOpen" :totalPrice="totalPrice" :vat-price="vatPrice" />
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-10">
    <Header @open-drawer="openDrawer" :total-price="1500" :totalPrice="totalPrice" />

    <div class="p-10">
      <router-view></router-view>
    </div>
  </div>
</template>

<style scoped></style>
