<script lang="ts">
import { defineComponent } from "vue";
import "../assets/board.scss";
import SquareFile from "./SquareFile.vue";
export default defineComponent({
  data() {
    const plays: Array<string> = null;
    const win: string | null = null;
    return {
      playerTurn: true,
      win,
      plays,
    };
  },
  methods: {
    alternateTurn() {
      // True is X
      // False is O
      this.playerTurn = !this.playerTurn;
    },
    winner() {
      const wincond = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ];
      
      for (let i = 0; i < 8; i++) {
        const [a, b, c] = wincond[i]
        
        if (
          this.plays[a] &&
          this.plays[a] === this.plays[b] &&
          this.plays[a] === this.plays[c]
        ) {
          console.log(wincond[i])
          this.win =  `Winner is ${this.plays[a]}`;
        }
      }
    },
    newGame(){
      this.plays = new Array(8).fill(null)
      this.win = null
    }
  },
  computed: {
    currentPlayer() {
      return this.playerTurn ? "X" : "O";
    },
    hasWinner() {
      if(!this.plays.includes(null)){
        this.win = 'Tie'
      }
      return this.win != null
    }
  },
  created(){
    this.newGame()
  },
  components: { SquareFile },
});
</script>

<template>
  <div class="board-page" v-if="!hasWinner">
    <h1 class="custom-span"> Playing: {{ currentPlayer }} </h1>
    <div class="board">
      <div v-for="range in 9" :key="range">
        <SquareFile
          @alternateTurn="alternateTurn"
          @winner="winner"
          :position="range-1"
          :player="currentPlayer"
          :plays="plays"
          :disabled="{disabled:hasWinner}"
        >
        </SquareFile>
      </div>
    </div>
  </div>
  <div class="game-over" v-else="hasWinner">
    <span class="custom-span"> {{win}} </span>
    <button 
    class="new-game"
    @click="newGame"
    > New Game </button>
  </div>
</template>
