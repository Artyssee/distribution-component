<script setup lang="ts">
import type { IProduct, ISegment } from '@/scripts/interfaces'
import { onMounted, ref, type Ref } from 'vue'
import InventorySystemRow from '../molecules/InventorySystemRow.vue'
import Products from '@/data/inventory.json'
import router from '@/router'

interface Props {
  productId: string
}

const props = defineProps<Props>()
const currentProduct = ref<IProduct>()
const amountInShoppingCart = ref(0)
const currentDate = ref<string>()

let segmentIndex: Ref<number> = ref(0)
let timeout: ReturnType<typeof setTimeout> = 0

// Filter out the correct product from the array and return the first item of it. Filtering out the segments with no quantity
const getProduct = (): IProduct => {
  let product = Products.filter((product: IProduct) => product.id === parseInt(props.productId))[0]
  const productSegments = product.segments.filter((segment: ISegment) => segment.quantity > 0);

  product.segments = productSegments;
  // Set the future segments
  //TODO: Set future segments. Somehow shift() not working

  return product
}

const previousPage = () => {
  router.push({ name: 'home' })
}

const handleUpdate = (e: Event) => {
  let difference: number
  let value = parseInt((e.target as HTMLInputElement).value)
  difference = 0
  clearTimeout(timeout)

  // Timeout to wait for large numbers
  timeout = setTimeout(() => {
    if (value && value > 0) {
      amountInShoppingCart.value += value
      value = 0

      // If the input is undefined, set it back to zero
      if (amountInShoppingCart.value === null) {
        amountInShoppingCart.value = 0
      }

      // TODO: Items are being removed, but the quantity being extracted is not being updated. Werking version with one segment at a time is on the previous commit
      if (currentProduct.value) {
        const calculatedValue =
          currentProduct.value.segments[segmentIndex.value].quantity - amountInShoppingCart.value

        if (calculatedValue <= 0) {
          difference = calculatedValue * -1
          currentProduct.value.segments[segmentIndex.value].quantity = 0
          segmentIndex.value++
          currentProduct.value.segments[segmentIndex.value].quantity -= difference
        } else {
          currentProduct.value.segments[segmentIndex.value].quantity = calculatedValue
        }

        currentDate.value = currentProduct.value.segments[segmentIndex.value].date
      }
    } else {
      return false
    }
  }, 500)
}

onMounted(() => {
  currentProduct.value = getProduct()
  currentDate.value = currentProduct.value.segments[segmentIndex.value].date
})
</script>

<template>
  <div class="inventorySystem">
    <template v-if="currentProduct">
        <h1 class="inventorySystem__heading">{{ currentProduct?.productName }}</h1>
        <!-- TODO: Makea this a a seperate component later-->
        <button class="inventorySystem__button" type="button" :onclick="previousPage">Return</button>
        <ul class="inventoryStatusList">
            <li class="inventorySatusList__item">
                <p><strong>Now:</strong></p>
                <p>{{ currentProduct?.segments[0].quantity }}</p>
            </li>
            <!-- TODO: Replace with futureSegments later -->
            <li v-for="(segment, index) in currentProduct.segments" class="inventorySatusList__item" :key="index">
                <p><strong>On: {{ segment.date }}</strong></p>
                <p>{{ segment.quantity }}</p>
            </li>
        </ul>
      <InventorySystemRow
        :ordered-products="amountInShoppingCart"
        :segments="currentProduct.segments"
      />
      <p>Will be delivered on {{ currentDate }}</p>
      <!-- TODO: Input should only accept numbers. text can still be inputted -->
      <p>
        Order amount: <input @input="(e: Event) => handleUpdate(e)" type="number" min="0" /> shoes
      </p>
    </template>
    <template v-else>
      <p>Current product is loading...</p>
    </template>
  </div>
</template>

<style lang="scss" scoped>
.inventorySystem {
  padding: 0 2rem;
}

.inventoryStatusList {
    display: flex;
    justify-content: space-between;
    list-style: none;
    flex-direction: row;
    flex-wrap: wrap;
    padding: 0;
    margin: 0;
}
</style>
