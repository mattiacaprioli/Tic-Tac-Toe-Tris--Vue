<template>
    <div class="flex flex-col items-center mb-8">
      <div v-for="(row, x) in board" :key="x" class="flex">
        <div
          v-for="(cell, y) in row"
          :key="y"
          @click="makeMove(x, y)"
          :class="`border border-black w-24 h-24 hover:bg-gray-300 flex items-center justify-center material-icons-outlined text-6xl cursor-pointer ${cell === 'X' ? 'text-blue-800' : 'text-red-800'}`"
        >
          {{ cell === 'X' ? 'close' : cell === 'O' ? 'circle' : '' }}
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: ['board'],
    methods: {
      makeMove(x, y) {
        this.$emit('move', x, y);
      },
    },
  };
  </script>