<script setup>
import { computed } from 'vue';

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number
});

const toPay = computed(() => props.totalPrice - props.vatPrice);
</script>

<template>
  <div class="flex flex-col gap-4 mt-7">
    <div class="flex gap-2">
      <span>Total:</span>
      <div class="flex-1 border-dashed border-b"></div>
      <b>${{ totalPrice }}</b>
    </div>
    <div class="flex gap-2">
      <span>Tax 5%:</span>
      <div class="flex-1 border-dashed border-b"></div>
      <b>${{ vatPrice }}</b>
    </div>
    <div class="flex gap-2">
      <span>To pay:</span>
      <div class="flex-1 border-dashed border-b"></div>
      <b>${{ toPay }}</b>
    </div>
    <button
      class="bg-lime-500 w-full mt-4 disabled:bg-slate-300 disabled:cursor-not-allowed cursor-pointer rounded-xl py-3 text-white hover:bg-lime-600 transition-colors active:bg-lime-700"
    >
      Checkout
    </button>
  </div>
</template>
