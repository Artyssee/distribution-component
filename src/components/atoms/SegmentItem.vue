<script setup lang="ts">
import type { ISegment } from '@/scripts/interfaces';

interface Props {
    segment: ISegment,
    segmentColor: string,
}

const props = defineProps<Props>();
</script>

<template>
    <li class="segment" :class="`segment--${props.segmentColor}`">
        <h2 class="segment__">On {{ props.segment.date }}</h2>
        <p>{{ props.segment.quantity }} units left in stock</p>
    </li>
</template>

<style lang="scss" scoped>
// Segment colors
$primary-segment: #A5D49E;
$secondary-segment: #9CB37F;
$tertiary-segment: #522A28;
$quaternary-segment: #C73E1C;
$senary-segment: #C59749;

.segment {
    list-style: none;

    &--primary {
        background-color: $primary-segment;
    }
    &--secondary {
        background-color: $secondary-segment;
    }
    &--tertiary {
        color: $primary-white;
        background-color: $tertiary-segment;
    }
    &--quartinary {
        background-color: $quaternary-segment;
    }
    &--senary {
        background-color: $senary-segment;
    }
}
</style>