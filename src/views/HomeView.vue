<script setup lang="ts">
import inventory from '@/data/inventory.json';
</script>

<template>
  <main>
    <ul>
      <li v-for="product in inventory" :key="product.id">
        <RouterLink to="/">{{ product.productName }}</RouterLink>
      </li>
    </ul>
  </main>
</template>
