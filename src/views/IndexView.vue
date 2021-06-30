<template>
  <div>

  </div>
</template>

<script>
export default {
  name: 'index-view',
  data() {
    return {
      intervalId: null,
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

        gamepad.buttons.forEach((button, id) => {
          if (button.pressed || button.touched) console.log(id, button);
        });
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
