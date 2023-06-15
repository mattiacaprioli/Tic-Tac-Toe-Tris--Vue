<template>
	<main class="pt-8 text-center">
	  <h1 class="mb-8 text-3xl font-bold uppercase">Tic Tac Toe</h1>
  
	  <h3 class="text-xl mb-4">Player {{ player }}'s Turn</h3>
  
	  <game-board :board="board" @move="makeMove"></game-board>
  
	  <game-status :winner="winner" :is-board-full="isBoardFull" @reset="resetGame" @back="goBack"></game-status>
	</main>
  </template>
  
  <script>
  import { ref, computed } from 'vue';
  import GameBoard from './components/GameBoard.vue';
  import GameStatus from './components/GameStatus.vue';
  
  export default {
	components: {
	  GameBoard,
	  GameStatus,
	},
	setup() {
	  const player = ref('X');
	  const board = ref([
		['', '', ''],
		['', '', ''],
		['', '', ''],
	  ]);
  
	  const moves = ref([]);
  
	  const calculateWinner = (board) => {
		const lines = [
		  [0, 1, 2],
		  [3, 4, 5],
		  [6, 7, 8],
		  [0, 3, 6],
		  [1, 4, 7],
		  [2, 5, 8],
		  [0, 4, 8],
		  [2, 4, 6],
		];
  
		for (let i = 0; i < lines.length; i++) {
		  const [a, b, c] = lines[i];
  
		  if (board[a] && board[a] === board[b] && board[a] === board[c]) {
			return board[a];
		  }
		}
  
		return null;
	  };
  
	  const winner = computed(() => calculateWinner(board.value.flat()));
  
	  const goBack = () => {
		if (moves.value.length === 0) return;
  
		const lastMove = moves.value.pop();
		board.value[lastMove.x][lastMove.y] = '';
		player.value = lastMove.player === 'X' ? 'O' : 'X';
	  };
  
	  const makeMove = (x, y) => {
		if (winner.value) return;
		if (board.value[x][y]) return;
  
		board.value[x][y] = player.value;
		player.value = player.value === 'X' ? 'O' : 'X';
  
		const move = {
		  player: player.value,
		  x,
		  y,
		};
		moves.value.push(move);
	  };
  
	  const isBoardFull = computed(() => {
		for (let row of board.value) {
		  for (let cell of row) {
			if (cell === '') {
			  return false;
			}
		  }
		}
		return true;
	  });
  
	  const resetGame = () => {
		board.value = [
		  ['', '', ''],
		  ['', '', ''],
		  ['', '', ''],
		];
		player.value = 'X';
		moves.value = [];
	  };
  
	  return {
		player,
		board,
		winner,
		isBoardFull,
		goBack,
		makeMove,
		resetGame,
	  };
	},
  };
  </script>
  
  <style>
  body {
	background-color: #8c8d8f00;
	color: black;
  }
  </style>