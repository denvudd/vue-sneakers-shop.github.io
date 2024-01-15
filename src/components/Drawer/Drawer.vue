<script setup>
import DrawerHead from './DrawerHead.vue';
import CartItemList from '../CartItemList.vue';
import CartTotal from '../CartTotal.vue';
import InfoBlock from '../InfoBlock.vue';
import { inject } from 'vue';

defineProps({
  totalPrice: Number,
  vatPrice: Number
});

const { closeDrawer } = inject('cart');
</script>

<template>
  <div @click="closeDrawer" class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div
    class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8 flex flex-col flex-1 justify-between"
  >
    <div>
      <DrawerHead />
      <InfoBlock
        v-if="!totalPrice"
        title="Cart is empty"
        description="Add some sneakers to your cart."
        image-url="/package-icon.png"
      />
      <CartItemList v-if="totalPrice" />
    </div>
    <CartTotal v-if="totalPrice" :total-price="totalPrice" :vat-price="vatPrice" />
  </div>
</template>
