<template>
  <div class="game">
    <h1>Next player: {{ status }}</h1>
    <div class="board">
      <div
        v-for="(square, index) in squares"
        class="square"
        :key="square"
        @click="handleSquareClick(square, index)"
      >
        {{ square }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return { squares: Array(9).fill(null), isPlayerOne: true, history: [{

    }] };
  },
  methods: {
    handleSquareClick(square, index) {
      // if there's a winner or if that square isn't null don't do anything
      if (this.calculateWinner() || square) {
        return;
      }

      const squares = [...this.squares];
      squares[index] = this.isPlayerOne ? "X" : "0";
      this.squares = squares;
      this.isPlayerOne = !this.isPlayerOne;
    },
    calculateWinner() {
      // possible winning combinations
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

      for (const line of lines) {
        const [a, b, c] = line;

        if (
          this.squares[a] &&
          this.squares[a] === this.squares[b] &&
          this.squares[a] === this.squares[c]
        ) {
          return this.squares[a];
        }
      }
      return null;
    },
  },
  computed: {
    status() {
      const winner = this.calculateWinner();

      if (winner) {
        return `Winner is ${winner}`;
      }
      return this.isPlayerOne ? "X" : "0";
    },
  },
};

/*
above board,there's the next player indicator
// create a div (board)
div should contain nine squares 
each square has the value
*/
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.game {
  /* margin: 10% auto; */
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 10%;
}

.board {
  display: grid;
  /* grid-template-columns: 1fr 1fr 1fr; */
  grid-template-columns: 100px 100px 100px;
  justify-content: center;
  /* width: 60%; */
  margin: 1rem auto;
}

.square {
  padding: 1rem;
  border: 1px solid black;
  height: 100px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 24px;
  font-weight: bold;
}
</style>
