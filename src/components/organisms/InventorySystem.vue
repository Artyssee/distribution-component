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
const currentDate = ref<string>();

let segmentIndex = 0;
let conditional = true;

// Filter out the correct product from the array and return the first item of it
const getProduct = (): IProduct => {
    return Products.filter((product: IProduct) => product.id === parseInt(props.productId))[0];
}

const previousPage = () => {
    router.push({ name: 'home' })
}

const handleUpdate = () => {
    let difference: number;
    difference = 0;

    if (conditional) {
    // If the input is undefined, set it back to zero
    if (amountInShoppingCart.value === null) {
        amountInShoppingCart.value = 0;
    }

    if(currentProduct.value) {
        const calculatedValue = currentProduct.value.segments[segmentIndex].quantity - amountInShoppingCart.value;

        if (calculatedValue < 0) {
            difference = calculatedValue * -1;
            currentProduct.value.segments[segmentIndex].quantity = 0;
            segmentIndex++;
            currentProduct.value.segments[segmentIndex].quantity -= difference;
        } else {
            currentProduct.value.segments[segmentIndex].quantity -= calculatedValue;
        }

        currentDate.value = currentProduct.value.segments[segmentIndex].date;
    }
    } else {
        return false;
    }
}

onMounted(() => {
    currentProduct.value = getProduct();
    currentDate.value = currentProduct.value.segments[segmentIndex].date;
})
</script>

<template>
        <h1>Inventory System component {{ props.productId }}</h1>
        <button type="button" :onclick="previousPage">Return</button>
        <p>In shoppingcart: {{ amountInShoppingCart }}</p>
        <template v-if="currentProduct">
            <p>Will be delivered on {{ currentDate }}</p>
            <input v-model="amountInShoppingCart" @input="handleUpdate" type="number" min="0" />
            <InventorySystemRow :ordered-products="amountInShoppingCart" :segments="currentProduct.segments" />
        </template>
        <template v-else>
            <p>Current product is loading...</p>
        </template>
</template>