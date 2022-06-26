<script setup>
import { storeToRefs } from "pinia";
import { useRoute, useRouter } from "vue-router";
import { useProductsStore } from "../stores/products";
import InnerHeader from "../components/InnerHeader.vue";
import LoadingError from "../components/LoadingError.vue";
import { watch } from "vue";
const { product, loading, error } = storeToRefs(useProductsStore());
const { fetchProduct } = useProductsStore();

const route = useRoute();
const router = useRouter();
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
              class="text-xl mt-4 p-3 border-2 border-rose-400 border-opacity-50 rounded-xl transition-all text-rose-600 hover:bg-rose-500 hover:border-opacity-100 hover:text-slate-50 disabled:opacity-30 disabled:pointer-events-none disabled:border-slate-800 disabled:text-slate-800"
              :disabled="!product.inStock"
            >
              <i class="fa-duotone fa-cart-circle-arrow-down"></i>
            </button>
          </div>
        </div>
      </div>
      <router-link to="/product/amazon-boxer"> boxer </router-link>
    </div>
  </div>
</template>
