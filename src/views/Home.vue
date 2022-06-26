<script setup>
import Logo from "../components/Logo.vue";
</script>

<template>
  <div class="bg-slate-100 min-h-screen w-full flex">
    <div class="basis-1/2 hidden lg:grid place-items-center">
      <Logo
        color="text-rose-500"
        :text="true"
        size="text-9xl"
        text-size="text-5xl"
      />
    </div>
    <div
      class="basis-1/2 flex-grow bg-rose-500 grid place-items-center text-slate-100"
    >
      <div class="flex flex-col gap-2 p-4 items-center">
        <h1 class="text-2xl font-bold">Welcome to the RoseKiwi</h1>
        <p class="max-w-[350px]">
          Lorem ipsum dolor sit amet consectetur adipisicing elit. Magni
          excepturi quae repellat nulla molestias sapiente provident dolorum
          ducimus quaerat nisi.
        </p>
        <router-link
          to="/shop"
          class="text-2xl mt-4 py-3 px-8 border-4 border-slate-100 border-opacity-50 rounded-full transition-all hover:bg-rose-400 hover:border-opacity-100"
        >
          Let's go Shopping
        </router-link>
      </div>
    </div>
  </div>
</template>
