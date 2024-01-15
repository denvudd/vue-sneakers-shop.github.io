<script setup>
defineProps({
  totalPrice: Number
});

const emit = defineEmits(['openDrawer']);
</script>

<template>
  <header class="flex items-center justify-between border-b border-slate-100 px-10 py-8">
    <router-link to="/">
      <div class="flex items-center gap-4">
        <img src="/logo.png" alt="Logo" class="w-10" />
        <div class="">
          <h2 class="texl-xl font-bold uppercase">Vue Sneakers</h2>
          <p class="text-slate-500">The best sneakers shop</p>
        </div>
      </div>
    </router-link>
    <nav>
      <ul class="flex items-center gap-10">
        <li
          @click="() => emit('openDrawer')"
          class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer"
        >
          <img src="/cart.svg" alt="Cart" />
          <b>${{ totalPrice }}</b>
        </li>
        <li class="text-gray-500 hover:text-black cursor-pointer">
          <router-link to="/favorites" class="flex items-center gap-3">
            <img src="/heart.svg" alt="Favorites" />
            <span>Favorites</span>
          </router-link>
        </li>
        <li class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer">
          <img src="/profile.svg" alt="Profile" />
          <span>Profile</span>
        </li>
      </ul>
    </nav>
  </header>
</template>
