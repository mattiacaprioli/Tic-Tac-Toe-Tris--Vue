<script setup>
import { ref, computed } from 'vue'

const player = ref('X')
const board = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', '']
])

const moves = ref([]);  // Array per salvare le mosse effettuate

// Funzione per calcolare il vincitore
const CalculateWinner = (board) => {
  const lines = [[0, 1, 2],[3, 4, 5],[6, 7, 8],[0, 3, 6],[1, 4, 7],[2, 5, 8],[0, 4, 8],[2, 4, 6]]

  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i]

    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
      return board[a]
    }
  }

  return null
}

// Calcola il vincitore in base allo stato attuale del tabellone
const winner = computed(() => CalculateWinner(board.value.flat()))

// Funzione per tornare indietro di un turno
const GoBack = () => {
  if (moves.value.length === 0) return;

  // Rimuovi l'ultima mossa dallo storico
  const lastMove = moves.value.pop();

  // Annulla l'effetto dell'ultima mossa sul tabellone
  board.value[lastMove.x][lastMove.y] = '';

  // Cambia il turno del giocatore
  player.value = lastMove.player === 'X' ? 'O' : 'X';
};


// Funzione per effettuare una mossa
const MakeMove = (x, y) => {
	if (winner.value) return
	if (board.value[x][y]) return

	board.value[x][y] = player.value
	player.value = player.value === 'X' ? 'O' : 'X'

  // Salva la mossa nello storico
  const move = {
    player: player.value,
    x,
    y
  };
  moves.value.push(move);
}

// Funzione per resettare il gioco
const ResetGame = () => {
	board.value = [
		['', '', ''],
		['', '', ''],
		['', '', '']
	]
	player.value = 'X'
  moves.value = []
}


</script>

<template>
	<main class="pt-8 text-center">
		<h1 class="mb-8 text-3xl font-bold uppercase">Tic Tac Toe</h1>

		<h3 class="text-xl mb-4">Player {{ player }}'s Turn</h3>

    <!-- Tabellone del gioco -->
		<div class="flex flex-col items-center mb-8">
			<div 
				v-for="(row, x) in board" 
				:key="x"
				class="flex">
				<div 
					v-for="(cell, y) in row" 
					:key="y" 
					@click="MakeMove(x, y)" 
					:class="`border border-black w-24 h-24 hover:bg-gray-300 flex items-center justify-center material-icons-outlined text-6xl cursor-pointer ${cell === 'X' ? 'text-blue-800' : 'text-red-800'}`">
					{{ cell === 'X' ? 'close' : cell === 'O' ? 'circle' : '' }}
				</div>
			</div>
		</div>

    <!-- Visualizza il vincitore (se presente) -->
		<div class="text-center">
			<h2 v-if="winner" class="text-6xl font-bold mb-8">Player '{{ winner }}' wins!</h2>
			<button @click="ResetGame" class="px-8 mr-6 py-1 bg-red-600 text-white rounded uppercase font-bold hover:bg-red-500 duration-300">Reset</button>
      <button @click="GoBack" class="px-8 py-1 bg-blue-800 rounded text-white font-bold hover:bg-blue-600 transition-colors duration-300">Go Back</button>
		</div>

	</main>
</template>

<style>
body {
	background-color: #8c8d8f00;
  color: black;
}
</style>