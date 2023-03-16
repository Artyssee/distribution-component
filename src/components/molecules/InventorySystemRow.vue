<script setup lang="ts">
import type { ISegment } from '@/scripts/interfaces';
import { onUpdated } from 'vue';
import Segment from '../atoms/SegmentItem.vue';

interface Props {
    segments: ISegment[],
    orderedProducts: number,
}

const props = defineProps<Props>();

const segmentColors = ["primary", "secondary", "tertiary", "quartinary", "senary"];
let renderedSegmentColors: string[] = [];
let colorIndex = 0;

const getRenderedSegmentColors = () => {
    props.segments.map(() => {
    if(colorIndex > 4) {
        colorIndex = 0;
    }

    renderedSegmentColors.push(segmentColors[colorIndex])
    colorIndex++;
})
}

onUpdated(() => {
    if (props.orderedProducts != null) {
        props.segments[0].quantity = props.segments[0].quantity - props.orderedProducts;
    }
})

getRenderedSegmentColors();
</script>

<template>
    <ul class="segmentsList">
        <Segment v-for="(segment, index) in props.segments" :segment="segment" :segmentColor="`${renderedSegmentColors[index]}`" :key="index" />
    </ul>
</template>