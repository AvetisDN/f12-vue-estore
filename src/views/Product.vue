<script setup>
import { storeToRefs } from "pinia";
import { useRoute } from "vue-router";
import { useProductsStore } from "../stores/products";
import { useCartStore } from "../stores/cart";
import InnerHeader from "../components/InnerHeader.vue";
import LoadingError from "../components/LoadingError.vue";
import { watch } from "vue";
const { product, loading, error } = storeToRefs(useProductsStore());
const { fetchProduct } = useProductsStore();

const { addToCart, isInCart } = useCartStore();

const route = useRoute();

fetchProduct(route.params.slug);

watch(
  () => route.params.slug,
  () => {
    fetchProduct(route.params.slug);
  }
);
</script>

<template>
  <div class="bg-rose-500 min-h-screen w-full p-3 lg:p-5 2xl:p-6 flex">
    <div class="container bg-slate-100 rounded-2xl p-4 lg:p-6 shadow-lg">
      <InnerHeader :title="product ? product.name : 'Product'" />
      <LoadingError :loading="loading" :error="error" />

      <div class="flex flex-col lg:flex-row gap-3" v-if="product">
        <img
          :src="
            product.image && product.image.original
              ? product.image.original
              : '/images/default-product-image.png'
          "
          :alt="product.name"
          class="rounded-lg h-80"
        />
        <div class="flex flex-col gap-3">
          <h2 class="text-rose-500 text-xl">
            {{ product.name }}
          </h2>
          <h4
            class="text-lg"
            :class="{
              'text-green-500': product.inStock,
              'text-red-500': !product.inStock,
            }"
          >
            {{ product.inStock ? "In Stock" : "Not In Stock" }}
          </h4>
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
              <i
                class="fa-solid fa-cart-circle-check text-emerald-600"
                v-else
              ></i>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
