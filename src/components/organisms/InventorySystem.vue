<script setup lang="ts">
import type { IProduct } from '@/scripts/interfaces';
import { onMounted, ref } from 'vue';
import InventorySystemRow from '../molecules/InventorySystemRow.vue';
import Products from '@/data/inventory.json';
import router from "@/router";

interface Props {
    productId: string,
}

const props = defineProps<Props>();
const currentProduct = ref<IProduct>();
const amountInShoppingCart = ref(0);

// Filter out the correct product from the array and return the first item of it
const getProduct = (): IProduct => {
    return Products.filter((product: IProduct) => product.id === parseInt(props.productId))[0];
}

const previousPage = () => {
    router.push({ name: 'home' })
}

onMounted(() => {
    currentProduct.value = getProduct();
})
</script>

<template>
        <h1>Inventory System component {{ props.productId }}</h1>
        <button type="button" :onclick="previousPage">Return</button>
        <p>In shoppingcart: {{ amountInShoppingCart }}</p>
        <template v-if="currentProduct">
            <input v-model="amountInShoppingCart" type="number" min="0" />
            <InventorySystemRow :ordered-products="amountInShoppingCart" :segments="currentProduct.segments" />
        </template>
        <template v-else>
            <p>Current product is loading...</p>
        </template>
</template>