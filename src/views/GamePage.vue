<!-- src/views/GamePage.vue -->
<template>
    <v-container>
      <v-row align="center" justify="center">
        <v-col cols="12" md="6">
          <v-text-field v-model.number="sizeX" label="Size X"></v-text-field>
          <v-text-field v-model.number="sizeY" label="Size Y"></v-text-field>
        </v-col>
      </v-row>
      <div class="grid">
        <div
          v-for="(row, rowIndex) in sizeY"
          :key="`row-${rowIndex}`"
          class="row"
        >
          <div
            v-for="(col, colIndex) in sizeX"
            :key="`col-${colIndex}`"
            class="square"
            :class="{ blue: grid[rowIndex][colIndex] }"
            @mouseover="toggleColor(rowIndex, colIndex)"
          ></div>
        </div>
      </div>
    </v-container>
  </template>
  
  <script>
  import { ref, reactive, watch } from 'vue';
  
  export default {
    name: 'GamePage',
    setup() {
      const sizeX = ref(10);
      const sizeY = ref(10);
      const grid = reactive([]);
  
      const initializeGrid = () => {
        grid.length = 0;
        for (let i = 0; i < sizeY.value; i++) {
          grid[i] = [];
          for (let j = 0; j < sizeX.value; j++) {
            grid[i][j] = false;
          }
        }
      };
  
      watch([sizeX, sizeY], initializeGrid, { immediate: true });
  
      const toggleColor = (row, col) => {
        grid[row][col] = !grid[row][col];
      };
  
      return { sizeX, sizeY, grid, toggleColor };
    }
  };
  </script>
  
  <style>
  .grid {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 20px;
  }
  .row {
    display: flex;
  }
  .square {
    width: 36px;
    height: 36px;
    background-color: white;
    margin: 1px;
    transition: background-color 0.3s;
    border: 1px solid black; /* Додаємо бордер */
  }
  .square.blue {
    background-color: blue;
  }
  </style>
  