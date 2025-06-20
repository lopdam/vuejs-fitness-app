<script setup lang="ts">
  import Welcome from './components/pages/Welcome.vue';
  import Layout from './components/layouts/Layout.vue';
  import Dashboard from './components/pages/Dashboard.vue';
  import Wrokout from './components/pages/Wrokout.vue';

  const selectedDisplay = 1;

</script>

<template>
  <Layout> 

    <Welcome v-if="selectedDisplay == 1"/>

    <!-- <Dashboard v-if="selectedDisplay == 2"/>

    <Wrokout v-if="selectedDisplay == 3"/> -->
  </Layout>
</template>

<style scoped>
</style>
