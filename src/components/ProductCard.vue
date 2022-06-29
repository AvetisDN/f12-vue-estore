<script setup>
import { useCartStore } from "../stores/cart";
const { addToCart, isInCart } = useCartStore();
defineProps(["product"]);
</script>

<template>
  <div
    class="bg-slate-50 p-3 rounded-xl border border-slate-300 transition-colors hover:border-slate-700 flex flex-col gap-3"
  >
    <router-link :to="`/product/${product.slug}`">
      <img
        :src="
          product.image && product.image.thumbnail
            ? product.image.thumbnail
            : '/images/default-product-image.png'
        "
        :alt="product.name"
        class="rounded-lg h-64 w-full object-cover"
      />
    </router-link>
    <router-link :to="`/product/${product.slug}`">
      <h2 class="text-rose-500 text-xl mb-auto">
        {{ product.name }}
      </h2>
    </router-link>
    <p class="text-slate-500 font-light text-sm">
      {{ product.description }}
    </p>
    <div class="flex justify-between pb-1 items-center">
      <div class="flex flex-col">
        <span class="line-through" v-if="product.oldPrice">
          ${{ product.oldPrice.toFixed(2) }}
        </span>
        <span class="text-lg text-rose-500 -my-2">
          ${{ product.price.toFixed(2) }}
        </span>
      </div>
      <button
        class="text-xl mt-4 p-3 border-2 border-rose-400 border-opacity-50 rounded-xl transition-all text-rose-600 hover:bg-rose-500 hover:border-opacity-100 hover:text-slate-50 disabled:opacity-30 disabled:pointer-events-none"
        :class="{
          'border-slate-700': !product.inStock,
          'border-emerald-600': isInCart(product.slug) !== -1,
        }"
        :disabled="!product.inStock || isInCart(product.slug) !== -1"
        @click="addToCart(product.slug)"
      >
        <i
          class="fa-duotone fa-cart-circle-arrow-down"
          v-if="isInCart(product.slug) === -1 && product.inStock"
        ></i>
        <i
          class="fa-solid fa-cart-circle-xmark text-slate-700"
          v-else-if="!product.inStock"
        ></i>
        <i class="fa-solid fa-cart-circle-check text-emerald-600" v-else></i>
      </button>
    </div>
  </div>
</template>
