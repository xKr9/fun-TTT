<script setup lang="ts">
import { ref } from "vue";
import SubGrid from "./components/SubGrid.vue";

const playerTurn = ref<string>("X");
const scoredGrids = ref(Array(9).fill(null));
const currentGridInPlay = ref<number | null>(null);
const subGrids = ref<(string | null)[][]>([
  Array(9).fill(null),
  Array(9).fill(null),
  Array(9).fill(null),
  Array(9).fill(null),
  Array(9).fill(null),
  Array(9).fill(null),
  Array(9).fill(null),
  Array(9).fill(null),
  Array(9).fill(null),
]);

const updateSubGrid = (value: number, subGrid: number) => {
  subGrids.value[subGrid][value] = playerTurn.value;
  playerTurn.value = playerTurn.value === "X" ? "O" : "X";
  if (scoredGrids.value[value]) {
    currentGridInPlay.value = null;
    return;
  }
  currentGridInPlay.value = value;
};

const updateGrid = (value: number) => {
  scoredGrids.value[value] = playerTurn.value === "X" ? "O" : "X";
};
</script>

<template>
  <div class="min-h-screen p-5 w-full flex flex-col">
    <div class="grid grid-cols-3 gap-2 bg-black">
      <SubGrid
        @scoredGrid="() => updateGrid(idx)"
        :scored="scoredGrids[idx]"
        :currentlyInPlay="
          currentGridInPlay === idx || currentGridInPlay === null
        "
        :gridValues="subGrids[idx]"
        @click="(value) => updateSubGrid(value, idx)"
        v-for="(_grid, idx) in subGrids"
      />
    </div>
  </div>
</template>
