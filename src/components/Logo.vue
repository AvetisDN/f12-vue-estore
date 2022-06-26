<script setup>
defineProps({
  color: {
    type: String,
    default: "text-slate-100",
  },
  size: {
    type: String,
    default: "text-xl",
  },
  text: {
    type: Boolean,
    default: true,
  },
  textSize: {
    type: String,
    default: "text-lg",
  },
});
</script>

<template>
  <div class="flex flex-col gap-0 items-center">
    <div :class="[color, size]">
      <i class="fad fa-kiwi-bird"></i>
    </div>
    <div
      v-if="text"
      class="font-bold flex tracking-tight"
      :class="[color, textSize]"
    >
      rose
      <span class="opacity-50">kiwi</span>
    </div>
  </div>
</template>
