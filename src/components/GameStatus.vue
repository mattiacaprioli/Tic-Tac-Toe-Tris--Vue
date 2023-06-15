<template>
    <div class="text-center">
      <h2 v-if="winner" class="text-6xl font-bold mb-8">Player '{{ winner }}' wins!</h2>
      <h2 v-else-if="isBoardFull" class="text-6xl font-bold mb-8">It's a tie!</h2>
      <button
        @click="resetGame"
        class="px-8 mr-6 py-1 bg-red-600 text-white rounded uppercase font-bold hover:bg-red-500 duration-300"
      >
        Reset
      </button>
      <button
        @click="goBack"
        class="px-8 py-1 bg-blue-800 rounded text-white font-bold hover:bg-blue-600 transition-colors duration-300"
      >
        Go Back
      </button>
    </div>
  </template>
  
  <script>
  export default {
    props: ['winner', 'isBoardFull'],
    methods: {
      resetGame() {
        this.$emit('reset');
      },
      goBack() {
        this.$emit('back');
      },
    },
  };
  </script>