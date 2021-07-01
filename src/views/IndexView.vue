<template>
  <div>
    <h1>{{ title }}</h1>
    <div
      class="game-controller__container"
    >
      <!-- <div
        v-for="(item, index) in axes"
        :key="index"
      >
        {{ index }}: {{ axes[index] }}
      </div> -->
      <game-controller
        class="game-controller"
        :active-buttons="activeButtons"
      />
      <game-controller-circle-pad-group
        :active-buttons="activeButtons"
        :axes="axes"
      />
    </div>
  </div>
</template>

<script>
import GameController from '../components/GameController.vue';
import GameControllerCirclePadGroup from '../components/GameControllerCirclePadGroup.vue';

export default {
  name: 'index-view',
  components: {
    GameController,
    GameControllerCirclePadGroup,
  },
  data() {
    return {
      axes: [],
      activeButtons: [],
      connected: false,
    };
  },
  computed: {
    title() {
      return this.connected ? 'Controller is connected' : 'Controller is not connected';
    },
  },
  created() {
    this.connect();
  },
  beforeDestroy() {
    this.disconnect();
  },
  methods: {
    connect() {
      window.addEventListener('gamepadconnected', this.onGamePadConnected);
      window.addEventListener('gamepaddisconnected', this.onGamePadDisconnected);
    },
    disconnect() {
      window.removeEventListener('gamepadconnected', this.onGamePadConnected);
      window.removeEventListener('gamepaddisconnected', this.onGamePadDisconnected);
    },
    onGamePadConnected(event) {
      this.connected = true;
      window.requestAnimationFrame((timestamp) => this.doPooling(event.gamepad.index, 0, timestamp));
    },
    onGamePadDisconnected() {
      this.connected = false;
      console.log('disconnected');
    },
    doPooling(index, previousTimestamp, currentTimestamp) {
      const elapsed = currentTimestamp - previousTimestamp;
      let nextTimestamp = previousTimestamp;

      if (this.connected && elapsed > 10) {
        const gamepad = navigator.getGamepads()[index];
        const activeButtons = [];

        for (let id = gamepad.buttons.length; id--;) {
          if (gamepad.buttons[id].pressed || gamepad.buttons[id].touched) activeButtons.push(id);
        }

        // console.log(this.activeButtons.toString(), activeButtons.toString(), this.activeButtons.toString() !== activeButtons.toString());
        if (this.activeButtons.length !== activeButtons.length || this.activeButtons.toString() !== activeButtons.toString()) {
          this.activeButtons = activeButtons;
        }
        // console.log(activeButtons);

        this.axes = gamepad.axes;
        nextTimestamp = currentTimestamp;
      }

      window.requestAnimationFrame((timestamp) => this.doPooling(index, nextTimestamp, timestamp));
    },
  },
};
</script>

<style lang="scss" scoped>
.game-controller {
  width: 500px;

  &__container {
    display: flex;
    align-items: center;
    flex-direction: column;
  }
}
</style>
