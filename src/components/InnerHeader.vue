<script setup>
defineProps({
  title: {
    type: String,
    default: "RoseKiwi Shop",
  },
});
</script>

<template>
  <header class="flex gap-5 text-slate-700 items-center mb-6">
    <button @click="$router.back()">
      <i class="fa-solid fa-arrow-left-long"></i>
    </button>
    <h3 class="text-xl font-bold">{{ title }}</h3>
    <button class="w-12 h-12 bg-slate-200 rounded-xl ml-auto">
      <i class="fa-solid fa-bars"></i>
    </button>
  </header>
</template>
