<script setup lang="ts">
import InventorySystemVue from '@/components/organisms/InventorySystem.vue';

interface Props {
    // Passing it via router makes it a string
    id: string,
}

const props = defineProps<Props>();
</script>

<template>
    <main>
        <InventorySystemVue :product-id="props.id" />
    </main>
</template>