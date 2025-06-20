<script setup lang="ts">
  import Welcome from './components/pages/Welcome.vue';
  import Layout from './components/layouts/Layout.vue';
  import Dashboard from './components/pages/Dashboard.vue';
  import Wrokout from './components/pages/Wrokout.vue';

  const selectedDisplay = 1;

</script>

<template>
  <Layout> 
    <!-- PAGE 1 -->
    <Welcome v-if="selectedDisplay == 1"/>
    <!-- PAGE 2 -->
    <Dashboard v-if="selectedDisplay == 2"/>
    <!-- PAGE 3 -->
    <Wrokout v-if="selectedDisplay == 3"/>
  </Layout>
</template>

<style scoped>
</style>
