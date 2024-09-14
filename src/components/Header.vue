<script setup>
defineProps({
  totalPrice: Number
})

const emit = defineEmits(['openDrawer'])
</script>

<template>
  <header class="flex justify-between border-b border-slate-200 px-10 py-1">
    <a href="https://t.me/snusmskvape/"
      ><div class="flex items-center gap-4">
        <img src="/logo.jpg" alt="Logo" class="w-32" />
        <div class="snus">
          <h2 class="text-xl font-bold uppercase">SNUS и точка</h2>
          <p class="text-slate-400">Вы сами все поняли.</p>
        </div>
      </div></a
    >

    <ul class="header_menu flex items-center gap-10">
      <li
        @click="() => emit('openDrawer')"
        class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black"
      >
        <img src="/cart.svg" alt="Cart" />
        <b>{{ totalPrice }} руб.</b>
      </li>

     
    </ul>
  </header>
</template>
