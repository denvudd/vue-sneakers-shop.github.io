<script setup>
defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number
});

const emit = defineEmits(['onClickRemove']);
</script>

<template>
  <div class="flex items-center border border-slate-100 p-4 rounded-xl gap-4 w-full">
    <img class="w-16 h-16" :src="imageUrl" :alt="title" />

    <div class="flex flex-col w-full">
      <p>{{ title }}</p>

      <div class="flex justify-between mt-2">
        <b class="flex-1">${{ price }}</b>
        <button @click="() => emit('onClickRemove')">
          <img
            src="/close.svg"
            alt="Close"
            class="opacity-40 hover:opacity-100 cursor-pointer transition-opacity"
          />
        </button>
      </div>
    </div>
  </div>
</template>
