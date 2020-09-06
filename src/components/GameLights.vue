<template>
  <div :class="`game-lights ${state}`">
      <input type="radio" name='pick' id='easyy' value="easy" v-model="picked" checked>
        <label for="easyy">Easy</label>
			<input type="radio" name="pick" id="normall" value="normal" v-model="picked">
        <label for="normall">Normal</label>
			<input type="radio" name='pick' id='hardd' value="hard" v-model="picked">
        <label for="hardd">Hard</label><br>
      <span>Выбрано: {{ picked }}</span>
      <gl-holder>
        <gl-button id='E' :shortkey="'1'" :color="'#FF5643'"  :sound="'1.mp3'"/>
        <gl-button id='I' :shortkey="'2'" :color="'dodgerblue'" :sound="'2.mp3'"/>
        <gl-button id='F' :shortkey="'3'" :color="'#FEEF33'" :sound="'3.mp3'"/>
        <gl-button id='J' :shortkey="'4'" :color="'#BEDE15'"  :sound="'4.mp3'"/>
        <game-over v-if="state == 'gameover'"/>



      </gl-holder>
  </div>
</template>

<script>

import GameOver from '@/components/GameLights/GameOver.vue'
import GlHolder from '@/components/GameLights/Holder.vue'
import GlButton from '@/components/GameLights/Button.vue'

export default {
  name: 'GameLights',
  
  components: {
    GlHolder,
    GlButton,
    GameOver,
  },
    data () {
    return {
      started: false,
      picked: '',
      checked: true,
      }
    },
  computed: {
    level: {
      get () {
        return this.$store.state.level
      },
      set (value) {
        this.$store.state.level = value
      }
    },
    lvl1:{
       get () {
        return this.$store.state.lvl1;
      },
    },
    lvl2:{
       get () {
        return this.$store.state.lvl2;
      },
    }, 
    lvl3:{
       get () {
        return this.$store.state.lvl3;
      },
    },  
    currentSequence: {
      get () {
        return this.$store.state.currentSequence
      },
      set (value) {
        this.$store.state.currentSequence = value
      }
    },
    state: {
      get () {
        return this.$store.state.state
      },
      set (value) {
        this.$store.state.state = value
      }
    }    
  },
  methods: {
    play (sequence = [],) {
        this.setState('waiting');
          sequence.forEach((n, i, a) => {
              if(this.picked=="easy"){
                a = this.$store.state.lvl1
                console.log(a) 
              }
              if(this.picked=="normal"){
                a = this.$store.state.lvl2
                console.log(a)  
                  }
              if(this.picked=="hard"){
                a = this.$store.state.lvl3
                console.log(a)  
              }
            setTimeout(() => {
              document.querySelectorAll(`[data-light-button]`)[n].click();
                if (i == sequence.length - 1) {
                  this.setState('listening');
                } 
              }, a * i); 
            });       
      },
    levelUp () {
      this.level++;
      this.currentSequence = [];
      for (let i=0; i<this.level; i++) {
        this.currentSequence.push(this.randomNumber(0,3));
      }
      this.play(this.currentSequence);
    },
    restart () {
      this.setState('waiting');
      this.$store.state.started = true;
      this.$store.state.hits = [];
      this.$store.state.level = 0;
      this.$store.state.elapsedTime = 0;
      this.$store.state.currentSequence = [];
      this.$store.state.sequenceListener = undefined;  
      window?.$gamelights_timer?.reset();     
      setTimeout(() => {
          window.$gamelights.levelUp();
      }, 500);      
    },       
    gameOver () {
      this.setState('gameover');
      this.$store.state.started = false;
    },
    randomNumber (min, max) {
      return Math.floor(Math.random() * (max - min + 1)) + min;
    },
    setState (state) {
      this.$store.state.state = state;
    },
    getState () {
      return this.$store.state.state;
    },
    sequence () {
      return this.currentSequence;
    },
    shiftSequence () {
      return this.currentSequence.shift();
    }
  },
  mounted () {
    window.$gamelights = this;
  }
}
</script>

<style scoped lang="scss">
#E {
  background: #FF5643;
	clip: rect(0px, 300px, 150px, 150px);
	width: 305px;
}
#I {
	background: dodgerblue;
	clip: rect(0px, 150px, 150px, 0px);
  width: 305px;
}
#F{
	background: #FEEF33;
	clip: rect(150px, 150px, 300px, 0px);
  width: 305px;
}
#J{
	background: #BEDE15;
	clip: rect(150px,300px, 300px, 150px);
  width: 305px;
}
  .game-lights {
    position: relative;
    margin-bottom: 40px;
    &.waiting {
      pointer-events: none;
    }
  }
</style>