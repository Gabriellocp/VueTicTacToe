<script lang="ts">
import "@/assets/square.scss";
import { defineComponent, type PropType } from "vue";
interface winner {
    disabled: boolean
}
export default defineComponent({
  emits: ["alternateTurn", "winner"],
  props: {
    position: Number,
    player: String || null,
    plays: Array,
    disabled: {
        type: Object as PropType<winner>
    }
  },
  data() {
    const marked: boolean | null = null;
    const currentPlayer: any = null;
    return {
      marked,
      currentPlayer,
    };
  },
  methods: {
    changeTurn() {
        this.plays[this.position] = this.currentPlayer
        this.marked = true;
        this.$emit("alternateTurn");
        this.$emit("winner")
    },
  },
  computed: {
    isAvailable() {
      return this.marked === null;
    },
    getPlayer: {
      get():string {
        return this.currentPlayer;
      },
      set(xoro: string):void {
        this.currentPlayer = xoro
        this.changeTurn();
      },
    },
    canClick(){
        // isAvailable = Can click div
        // Disabled = return TRUE if has a winner
        return this.isAvailable && !this.disabled.disabled
    }
  },
});
</script>

<template>
  <div
    @click="canClick ? (getPlayer = player) : null"
    :class="['square', canClick ? 'not-marked' : ''].join(' ')"
  >
    <span> {{ getPlayer }} </span>
  </div>
</template>
