<template>
  <div class="flex flex-col min-h-screen font-Roboto bg-weather-primary">
<SiteNav />
<RouterView />

  </div>
</template>

<script setup>
import { RouterLink } from 'vue-router';
import SiteNav from './components/SiteNav.vue';

</script>

<style lang="scss" scoped>

</style>