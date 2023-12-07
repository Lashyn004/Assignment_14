<template>
  <div>
    <h1>TIC TAC TOE</h1>
    <div class="game" :class="{ 'game-over': game_over_bool }">
      <span
        class="game__section"
        v-for="(item, index) in board"
        :key="index"
        @click="move(index)"
        :class="[
          { 'game__section--occupied': item !== '' },
          { 'game__section--winner': isWinnerCell(index) }
        ]"
      >
        {{ item }}
      </span>
    </div>
    <h3 v-if="game_over_bool">{{ message || 'Draw' }}</h3>
    <h3 v-else>{{ 'Turn ' + turn }}</h3>
    <button @click="restart">Restart</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      board: Array(9).fill(''),
      turn: 'X',
      message: null,
      game_over_bool: false,
      winnerCells: []
    };
  },
  methods: {
    move(index) {
      if (this.game_over_bool || this.board[index] !== '') return;

      this.board[index] = this.turn;
      this.game_over_bool = this.game_over();
      this.turn = this.turn === 'X' ? 'O' : 'X';
    },
    game_over() {
      const success_cases = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6]
      ];

      for (const s_case of success_cases) {
        const [a, b, c] = s_case;
        if (
          this.board[a] !== '' &&
          this.board[a] === this.board[b] &&
          this.board[a] === this.board[c]
        ) {
          this.message = 'The winner is player <' + this.turn + '>';
          this.winnerCells = s_case;
          return true;
        }
      }

      if (this.board.every((item) => item !== '')) {
        this.message = 'Draw';
        return true;
      }

      return false;
    },
    isWinnerCell(index) {
      return this.winnerCells.includes(index);
    },
    restart() {
      this.board = Array(9).fill('');
      this.turn = 'X';
      this.message = null;
      this.game_over_bool = false;
      this.winnerCells = [];
    }
  }
};
</script>

<style>
/* Add your existing styles here */

.game {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  width: 300px;
  max-width: 100%;
  margin: 20px auto;
}

.game__section {
  width: 100%;
  height: 100px;
  background-color: black;
  border: 1px #fff solid;
  opacity: 0.85;
  cursor: pointer;
  color: rgb(0, 255, 242);
  font-size: 64px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: opacity 0.3s ease-in-out;
}

.game__section--occupied {
  cursor: not-allowed;
}

.game__section--winner {
  background-color: rgb(17, 207, 17); /* Highlight the winning cells with a different color */
}

.game__section:hover {
  opacity: 1;
}

h1,
h3 {
  text-align: center;
  color: rgb(51, 51, 255);
}

h3 {
  font-size: 24px;
  color: rgb(17, 207, 17);
}

.game-over {
  pointer-events: none;
}
</style>
