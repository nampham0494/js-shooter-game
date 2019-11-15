<template>
  <div :style="getStyles" class="player"></div>
</template>

<script>

export default {
  name: 'Index',
  props: ['player'],
  computed: {
    top () {
      return this.player.position.top - 25
    },
    left () {
      return this.player.position.left - 25
    },
    angle () {
      return this.player.angle
    },
    getStyles () {
      return {
        top: this.top + 'px',
        left: this.left + 'px',
        transform: 'rotate(' + this.angle + 'deg)'
      }
    }
  }
}
</script>

<style lang="scss">
.player {
    width: 50px;
    height: 50px;
    z-index: 2;
    position: absolute;
    transition: all 0.1s ease;
    transition-property: top, left;
    background: url(../assets/images/player.png);
    background-size: contain;
}
</style>
