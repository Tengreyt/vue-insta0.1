<template>
  <div class="bg-white">
    <Header />
  </div>
  <Test/>
  <Test/>
  <div class="mt-5 px-3">
    <p v-for="(item, index) in items" :key="index">{{ item }}</p>
  </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Test from './components/Test.vue';



</script>

<style scoped>

</style>
