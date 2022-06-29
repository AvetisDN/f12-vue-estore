<script setup>
import { storeToRefs } from "pinia";
import { ref } from "vue";
import OverlayMenu from "./OverlayMenu.vue";
import OverlayShadow from "./OverlayShadow.vue";

import { useCartStore } from "../stores/cart";
import OverlayCart from "./OverlayCart.vue";
import { useRouter } from "vue-router";
const { cart, totalCount, totalPrice } = storeToRefs(useCartStore());

defineProps({
  title: {
    type: String,
    default: "RoseKiwi Shop",
  },
});

const showMenu = ref(false);
const showCart = ref(false);

const router = useRouter();

const closeOverlay = () => {
  showCart.value = false;
  showMenu.value = false;
  setTimeout(() => router.push("/cart"), 400);
};
</script>

<template>
  <header class="flex gap-5 text-slate-700 items-center mb-6">
    <router-link :to="$route.fullPath === '/shop' ? '/' : '/shop'">
      <i class="fa-solid fa-arrow-left-long"></i>
    </router-link>
    <h3 class="text-xl font-bold">{{ title }}</h3>
    <button
      class="w-12 h-12 bg-slate-200 rounded-xl ml-auto"
      @click="showMenu = !showMenu"
    >
      <i class="fa-solid fa-bars"></i>
    </button>
    <button
      class="relative w-12 h-12 bg-rose-500 text-rose-50 rounded-xl"
      @click="showCart = !showCart"
    >
      <i class="fa-solid fa-cart-shopping"></i>
      <span
        class="absolute w-6 h-6 bg-slate-600 rounded-full -top-[6px] -right-[6px] flex items-center justify-center text-xs font-bold leading-3 pb-[2px]"
      >
        {{ totalCount }}
      </span>
    </button>
    <Transition name="menu">
      <OverlayMenu v-if="showMenu" />
    </Transition>
    <Transition name="fade">
      <OverlayShadow
        v-if="showMenu || showCart"
        @click="
          showMenu = false;
          showCart = false;
        "
      />
    </Transition>
    <Transition name="slide-down">
      <OverlayCart
        v-if="showCart"
        :cart="cart"
        :totalPrice="totalPrice"
        @closeModal="closeOverlay"
      />
    </Transition>
  </header>
</template>

<style></style>
