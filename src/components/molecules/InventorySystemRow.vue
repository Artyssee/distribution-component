<script setup lang="ts">
import type { ISegment } from '@/scripts/interfaces';
import { onBeforeMount } from 'vue';
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

onBeforeMount(() => {
    getRenderedSegmentColors();
})
</script>

<template>
    <ul class="segmentsList">
        <Segment
            v-for="(segment, index) in props.segments"
            v-show="segment.quantity > 0"
            :segment="segment"
            :segmentColor="`${renderedSegmentColors[index]}`"
            :key="index"
        />
    </ul>
</template>