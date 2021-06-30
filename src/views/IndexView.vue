<template>
  <div>
    <game-controller
      :active-buttons="activeButtons"
    />
    <div
      v-for="(item, index) in axes"
      :key="index"
    >
      {{ axes[index] }}
    </div>
  </div>
</template>

<script>
import GameController from '../components/GameController.vue';

export default {
  components: {
    GameController,
  },
  name: 'index-view',
  data() {
    return {
      intervalId: null,
      axes: [],
      activeButtons: [],
    };
  },
  created() {
    window.addEventListener('gamepadconnected', this.onGamePadConnected);
    window.addEventListener('gamepaddisconnected', this.onGamePadDisconnected);
  },
  beforeDestroy() {
    window.removeEventListener('gamepadconnected', this.onGamePadConnected);
    window.removeEventListener('gamepaddisconnected', this.onGamePadDisconnected);
  },
  methods: {
    onGamePadConnected(event) {
      const { index } = event.gamepad;
      // All buttons and axes values can be accessed through
      this.intervalId = setInterval(() => {
        const gamepad = navigator.getGamepads()[index];

        // this.activeButtons = gamepad.buttons.findIndex((button) => button.pressed || button.touched);
        // console.log(this.activeButtons);
        // gamepad.buttons.forEach((button, id) => {
        //   if (button.pressed || button.touched) console.log(id, button);
        // });
        const activeButtons = [];
        for (let id = gamepad.buttons.length; id--;) {
          if (gamepad.buttons[id].pressed || gamepad.buttons[id].touched) activeButtons.push(id);
        }
        if (this.activeButtons.length !== activeButtons.length && this.activeButtons.toString() !== activeButtons.toString()) {
          this.activeButtons = activeButtons;
        }

        this.axes = gamepad.axes;
      }, 100);
    },
    onGamePadDisconnected() {
      console.log('disconnected');
    },
    cancelPolling() {
      if (this.intervalId !== null) {
        clearInterval(this.intervalId);
        this.intervalId = null;
      }
    },
  },
};
</script>

<style>

</style>
