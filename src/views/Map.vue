<template>
  <div class="map" @mousemove="cursorHandler" @click="shoot" ref="map">
    <player :player="player" v-bind:key="player.id"></player>
    <bullet :bullet="bullet" v-for="(bullet, key) in bullets" v-bind:key="key"></bullet>
  </div>
</template>

<script>

import Player from '@/components/Player'
import Bullet from '@/components/Bullet'
import BulletSound from '@/assets/sounds/shoot.mp3'
import FootstepsSound from '@/assets/sounds/footsteps.mp3'

export default {
  name: 'Index',
  data () {
    return {
      cursorPosition: {
        x: 0,
        y: 0
      },
      init: {
        mapWidth: 0,
        mapHeight: 0
      },
      player: {
        id: 1,
        angle: 0,
        position: {
          top: 250,
          left: 550
        }
      },
      bullets: [],
      sounds: []
    }
  },
  components: {
    Player,
    Bullet
  },
  methods: {
    shoot () {
      this.bullets.push({
        angle: this.player.angle,
        position: {
          top: this.player.position.top,
          left: this.player.position.left
        }
      })

      this.soundManager('shoot')
    },
    cursorHandler (event) {
      this.cursorPosition.x = event.clientX
      this.cursorPosition.y = event.clientY
      this.player.angle = Math.atan2(this.cursorPosition.x - this.player.position.left + 25, -(this.cursorPosition.y - this.player.position.top + 25)) * (180 / Math.PI)
    },
    moveBullets () {
      this.bullets.forEach(bullet => {
        if (bullet.position.top >= -5 && bullet.position.left >= -5 && bullet.position.top <= this.init.mapHeight + 5 && bullet.position.left <= this.init.mapWidth + 5) {
          bullet.position.top += Math.cos(-bullet.angle * Math.PI / 180) * -5
          bullet.position.left += Math.sin(-bullet.angle * Math.PI / 180) * -5
        }
      })
    },
    onKeyPress () {
      window.addEventListener('keypress', (e) => {
        let angle = Math.atan2(this.cursorPosition.x - this.player.position.left + 25, -(this.cursorPosition.y - this.player.position.top + 25)) * (180 / Math.PI)
        if (e.key === 'w') {
          this.player.position.top += Math.cos(-angle * Math.PI / 180) * -10
          this.player.position.left += Math.sin(-angle * Math.PI / 180) * -10
        } else if (e.key === 's') {
          this.player.position.top += Math.cos(-angle * Math.PI / 180) * 10
          this.player.position.left += Math.sin(-angle * Math.PI / 180) * 10
        } else if (e.key === 'a') {
          this.player.position.top += Math.sin(angle * Math.PI / 180) * -10
          this.player.position.left += Math.cos(angle * Math.PI / 180) * -10
        } else if (e.key === 'd') {
          this.player.position.top += Math.sin(angle * Math.PI / 180) * 10
          this.player.position.left += Math.cos(angle * Math.PI / 180) * 10
        }
      })
    },
    tick () {
      let self = this
      setInterval(function () {
        self.moveBullets()
      }, 20)
    },
    initGame () {
      this.init.mapWidth = this.$refs['map'].offsetWidth
      this.init.mapHeight = this.$refs['map'].offsetHeight
      this.sounds['footsteps'] = new Audio(FootstepsSound)
    },
    soundManager (action) {
      if (action === 'shoot') {
        new Audio(BulletSound).play()
      } else if (action === 'footsteps') {
        this.sounds['footsteps'].play()
      }
    }
  },
  mounted () {
    this.tick()
    this.initGame()
    this.onKeyPress()
  }
}
</script>

<style lang="scss">
.map {
    width: 100%;
    height: 100vh;
    overflow: hidden;
    position: relative;
    background: url(../assets/images/grass.jpg);
}
</style>
