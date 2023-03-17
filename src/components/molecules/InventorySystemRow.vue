<script setup lang="ts">
import type { ISegment } from '@/scripts/interfaces'
import { onBeforeMount } from 'vue'
import Segment from '../atoms/SegmentItem.vue'

interface Props {
  segments: ISegment[]
  orderedProducts: number
}

const props = defineProps<Props>()
const segmentColors = ['primary', 'secondary', 'tertiary', 'quartinary', 'senary']
let renderedSegmentColors: string[] = []
let colorIndex = 0

const getRenderedSegmentColors = () => {
  props.segments.map(() => {
    if (colorIndex > 4) {
      colorIndex = 0
    }

    renderedSegmentColors.push(segmentColors[colorIndex])
    colorIndex++
  })
}

onBeforeMount(() => {
  getRenderedSegmentColors()
})

</script>

<template>
  <ul class="segmentsList">
    <!-- TODO: Find a trick for animation each segment. Passing the isHidden prop for now. Not used yet -->
    <Segment
      v-for="(segment, index) in props.segments"
      :isHidden="false"
      :segment="segment"
      :index="index"
      :segmentColor="`${renderedSegmentColors[index]}`"
      :key="index"
    />
  </ul>
</template>

<style lang="scss" scoped>
.segmentsList {
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: 1fr;
  gap: 20px;
  padding: 0;

  @include breakpoint('md') {
    grid-template-columns: 1fr 1fr;
  }

  @include breakpoint('lg') {
    grid-template-columns: repeat(3, 1fr);
  }
}
</style>
