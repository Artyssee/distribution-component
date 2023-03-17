<script setup lang="ts">
import type { ISegment } from '@/scripts/interfaces'
import { ref } from 'vue';

interface Props {
  segment: ISegment
  segmentColor: string
  index: number
  isHidden: boolean
}

const props = defineProps<Props>()
const showModal = ref(false);

const setShowModal = () => {
    showModal.value = !showModal.value;
}
</script>

<template>
  <Transition name="fade">
    <li v-if="props.segment.quantity > 0 && !isHidden" class="segment" :class="`segment--${props.segmentColor} ${isHidden ? '' : ''}`">
        <Transition name="fade">
            <div v-if="index != 0 && showModal" class="segmentPopup">
                <p>Available on: {{ props.segment.date }}</p>
            </div>
            <div class="segmentPopup" v-else-if="index === 0">
                <p>Available Now!</p>
            </div>
        </Transition>
        <p v-on:mouseover="setShowModal" v-on:mouseleave="setShowModal" class="segment__body">{{ props.segment.quantity }} units left in stock</p>
    </li>
  </Transition>
</template>

<style lang="scss" scoped>
// Segment colors
$primary-segment: #a5d49e;
$secondary-segment: #9cb37f;
$tertiary-segment: #522a28;
$quaternary-segment: #c73e1c;
$senary-segment: #c59749;

.segment {
  list-style: none;
  padding: 0.8rem;
  border: 1px solid $primary-black;
  position: relative;

  &__body {
    color: $primary-black;
  }

  &--primary {
    background-color: $primary-segment;
  }
  &--secondary {
    background-color: $secondary-segment;
  }
  &--tertiary {
    color: $primary-white;
    background-color: $tertiary-segment;

    .segment__heading, .segment__body {
        color: $primary-white;
    }
  }

  &--quartinary {
    background-color: $quaternary-segment;
    color: $primary-white;

    .segment__heading, .segment__body {
        color: $primary-white;
    }
  }

  &--senary {
    background-color: $senary-segment;
  }
}

.segmentPopup {
    position: absolute;
    display: flex;
    align-items: center;
    width: 200px;
    height: auto;
    background-color: $primary-white;
    border: 1px solid $primary-black;
    padding: 1.2rem;
    z-index: 1;
    top: -16px;
    right: 8px;
    padding: 0 2rem;

    &__text {
        color: $primary-black;
    }
}

// Vue transition for fading out segments
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease-in-out;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
