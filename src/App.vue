<template>
  <div id="app">
    <h1>X and O Game</h1>
    <div class="board">
      <div
        v-for="(cell, index) in board"
        :key="index"
        class="cell"
        @click="makeMove(index)"
      >
        <span :class="{ X: cell === 'X', O: cell === 'O' }">{{ cell }}</span>
      </div>
    </div>
    <div v-if="winner" class="result">
      <h2>Winner: {{ winner }}</h2>
      <button @click="resetGame">Play Again</button>
    </div>
    <div v-else-if="isBoardFull" class="result">
      <h2>It's a Draw!</h2>
      <button @click="resetGame">Play Again</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      board: Array(9).fill(null),
      currentPlayer: "X",
      winner: null,
    };
  },
  computed: {
    isBoardFull() {
      return this.board.every((cell) => cell !== null);
    },
  },
  methods: {
    makeMove(index) {
      if (!this.board[index] && !this.winner) {
        this.$set(this.board, index, this.currentPlayer);
        if (this.checkWinner()) {
          this.winner = this.currentPlayer;
        } else {
          this.currentPlayer = this.currentPlayer === "X" ? "O" : "X";
        }
      }
    },
    checkWinner() {
      const winningCombinations = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ];

      for (let combination of winningCombinations) {
        const [a, b, c] = combination;
        if (
          this.board[a] &&
          this.board[a] === this.board[b] &&
          this.board[a] === this.board[c]
        ) {
          return true;
        }
      }
      return false;
    },
    resetGame() {
      this.board = Array(9).fill(null);
      this.currentPlayer = "X";
      this.winner = null;
    },
  },
};
</script>

<style scoped>
#app {
  font-family: "Arial", sans-serif;
  text-align: center;
  background-color: #f3f4f6;
  padding: 30px;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

h1 {
  font-size: 2.5rem;
  margin-bottom: 20px;
  color: #333;
  text-transform: uppercase;
  letter-spacing: 4px;
}

.board {
  display: grid;
  grid-template-columns: repeat(3, 120px);
  grid-template-rows: repeat(3, 120px);
  gap: 15px;
  justify-content: center;
}

.cell {
  width: 120px;
  height: 120px;
  background-color: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2.5rem;
  cursor: pointer;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.cell:hover {
  background-color: #e0f7fa;
  transform: scale(1.05);
}

.cell:active {
  transform: scale(0.95);
}

.X {
  color: #ef5350;
  font-weight: bold;
}

.O {
  color: #000000;
  font-weight: bold;
}

.result {
  margin-top: 20px;
}

.result h2 {
  font-size: 2rem;
  margin-bottom: 15px;
  color: #444;
}

button {
  padding: 10px 20px;
  font-size: 1rem;
  border: none;
  background-color: #42a5f5;
  color: #fff;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #1e88e5;
}

button:active {
  background-color: #1565c0;
}

body {
  margin: 0;
}
</style>
