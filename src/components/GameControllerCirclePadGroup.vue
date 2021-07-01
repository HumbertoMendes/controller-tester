<template>
  <div
    class="game-controller__circle-pad-group"
  >
    <game-controller-circle-pad
      :x-axis="axes[0]"
      :y-axis="axes[1]"
      :active="isButtonActive(L3)"
    />
    <game-controller-circle-pad
      :x-axis="axes[2]"
      :y-axis="axes[3]"
      :active="isButtonActive(R3)"
    />
  </div>
</template>

<script>
import GameControllerCirclePad from './GameControllerCirclePad.vue';
import { L3, R3 } from '../constants/KeysConstants';

export default {
  name: 'game-controller-circle-pad-group',
  components: {
    GameControllerCirclePad,
  },
  props: {
    activeButtons: {
      type: Array,
      required: true,
    },
    axes: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      L3,
      R3,
    };
  },
  methods: {
    isButtonActive(id) {
      return this.activeButtons.includes(id);
    },
    getFill(id) {
      console.log(id);
      return this.isButtonActive(id) ? 'red' : 'transparent';
    },
  },
};
</script>

<style lang="scss" scoped>
.game-controller {
  &__circle-pad-group {
    width: 300px;
    display: flex;
    justify-content: space-around;
  }
}
</style>
