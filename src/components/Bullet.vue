<template>
  <div :style="getStyles" class="bullet"></div>
</template>

<script>

export default {
  name: 'Index',
  props: ['bullet'],
  computed: {
    top () { return this.bullet.position.top - 2 },
    left () { return this.bullet.position.left - 2 },
    getStyles () {
      return {
        top: this.top + 'px',
        left: this.left + 'px'
      }
    }
  }
}
</script>

<style lang="scss">
.bullet {
    width: 4px;
    height: 4px;
    border-radius: 50%;
    background: black;
    position: absolute;
}
</style>
