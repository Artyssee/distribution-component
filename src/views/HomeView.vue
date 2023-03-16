<script setup lang="ts">
import inventory from '@/data/inventory.json';
</script>

<template>
  <main>
    <ul>
      <li v-for="product in inventory" :key="product.id">
        <RouterLink :to="{name: 'product', params: { id: product.id }}">{{ product.productName }}</RouterLink>
      </li>
    </ul>
  </main>
</template>
