<script setup></script>

<template>
  <div class="w-full min-h-screen relative">
    <router-view v-slot="{ Component, route }">
      <transition :name="route.meta.transitionName" mode="default">
        <component :is="Component" />
      </transition>
    </router-view>
  </div>
</template>
