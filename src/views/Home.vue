<template>
  <div class="home">
    <game-bg/>
    <div class="container">
      <div class="game-header">
        <game-logo/>
        <game-level/>
      </div>
      <game-lights msg="Simon game"/>
      <game-menu/>
    </div>
  </div>
</template>

<script>

import GameBg from '@/components/GameBg.vue'
import GameLogo from '@/components/GameLogo.vue'
import GameLights from '@/components/GameLights.vue'
import GameMenu from '@/components/GameMenu.vue'
import GameLevel from '@/components/GameMenu/GameLevel.vue'


export default {
  name: 'Home',
  components: {
    GameBg,
    GameLogo,
    GameMenu,
    GameLights,
    GameLevel,

  },
  methods: {
    lvl(){
      this.$emit('picked')
      console.log(this.picked)
    }
  }
}
</script>

<style lang="scss">
.home {
  padding: 8px;
  .game-header {
    display: flex;
    align-items: center;
    margin-top: 20px;
    justify-content: space-between;
  }
}
</style>
