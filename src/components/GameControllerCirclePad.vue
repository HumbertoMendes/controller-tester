<template>
  <div
    class="game-controller__circle-pad"
  >
    <div
      class="game-controller__circle-pad__deadzone"
      :style="`left: ${xPosition}%; top: ${yPosition}%;`"
    />
  </div>
</template>

<script>
export default {
  name: 'game-controller-circle-pad',
  props: {
    xAxis: {
      type: Number,
      default: 0,
    },
    yAxis: {
      type: Number,
      default: 0,
    },
  },
  computed: {
    xPosition() {
      return 50 + (50 * this.xAxis);
    },
    yPosition() {
      return 50 + (50 * this.yAxis);
    },
  },
};
</script>

<style lang="scss" scoped>
.game-controller__circle-pad {
  width: 100px;
  height: 100px;
  border: 5px solid black;
  border-radius: 50%;
  position: relative;

  &__deadzone {
    width: 10px;
    height: 10px;
    background-color: black;
    border-radius: 50%;
    position: absolute;
    transform: translate(-50%, -50%);
  }
}
</style>
