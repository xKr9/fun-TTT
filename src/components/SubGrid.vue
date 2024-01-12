<script setup lang="ts">
import { defineProps } from "vue";
import { WINNING_COMBINATIONS } from "../utils/constants";

const props = defineProps<{
  gridValues: string[];
  currentlyInPlay: boolean;
  scored: string | null;
}>();

const emit = defineEmits<{
  click: [value: number];
  scoredGrid: [];
}>();

const updateGrid = (value: number) => {
  if (props.scored) return;
  if (props.gridValues[value]) return;
  emit("click", value);
  WINNING_COMBINATIONS.forEach((combination) => {
    const val1 = props.gridValues[combination[0]];
    const val2 = props.gridValues[combination[1]];
    const val3 = props.gridValues[combination[2]];
    if (
      val1 === val2 &&
      val2 === val3 &&
      val3 === val1 &&
      val1 &&
      val2 &&
      val3
    ) {
      emit("scoredGrid");
    }
  });
};
</script>

<template>
  <div class="grid relative grid-cols-3 gap-1 bg-gray-400">
    <div
      v-if="!currentlyInPlay"
      class="bg-black absolute inset-0 opacity-30 h-full w-full"
    ></div>
    <h1
      v-if="scored"
      class="text-9xl text-white absolute top-1/2 z-10 -translate-y-1/2 -translate-x-1/2 left-1/2 opacity-100"
    >
      {{ scored }}
    </h1>
    <div
      v-if="scored"
      class="bg-black flex items-center justify-center absolute inset-0 opacity-50 h-full w-full"
    ></div>
    <button
      @click="updateGrid(idx)"
      v-for="(value, idx) in props.gridValues"
      :key="idx"
      class="w-full text-2xl font-bold h-20 bg-white"
      :class="{
        'text-green-500': value === 'X',
        'text-red-500': value === 'O',
      }"
    >
      {{ value }}
    </button>
  </div>
</template>
