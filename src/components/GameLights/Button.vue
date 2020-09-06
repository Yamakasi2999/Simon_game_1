<template>
  <div class="game-lights-button">
    <button ref="btn" :style="`background-color:${color}`" @click='click($event)' :data-light-button="shortkey">
        {{shortkey}}
    </button>  
  </div>
</template>
<script>
export default {
  name: 'LightsButton',
  props: {   
    shortkey: {
      type: String,
      required: true
    },
    color: {
      type: String,
      required: true
    },
    sound: {
      type: String,
      required: true
    }
  }, 
  methods: {
    click ($event) {
      const turn = this.$store.state.state;
      if (turn != 'listening' && (!$event || $event.isTrusted)) {
        // if user triggered an action when is not its turn
        // the action will be ignored. 
        return;
      }
      const $btn = this.$refs.btn;
      const key = $btn?.dataset?.lightButton.toLowerCase();
      $btn.classList.add('click');
      setTimeout(() => { $btn.classList.remove('click'); }, 200);
      this.playSound(require(`@/assets/sounds/${this.sound}`));
      this.checkSequence(key);
    },
    checkSequence (key) {
      if (window.$gamelights.getState() === 'listening') {
        this.$store.state.hits.push(key);
        const keymap = {"1": 0, "2": 1, "3": 2, "4": 3};
        const keyindex = keymap[key.toLowerCase()];
        const expected = window.$gamelights.shiftSequence();
        if (expected == keyindex) {
          if (!window.$gamelights.sequence().length) {
            setTimeout(() => {
              if (this.$store.state.state == "gameover") return;
              window.$gamelights.setState('waiting');
              window.$gamelights.levelUp();
            }, 1000);
          }
        } else {
          window.$gamelights.gameOver();
        }
      }
    },
    playSound (sound) {
      const audio = new Audio(sound);
      audio.play();
    }    
  },
  mounted () {
    document.addEventListener('keypress', e => {
      const shortkey = this.shortkey.toLowerCase();
      if (e.key.toLowerCase() == shortkey || e.code.toLowerCase() == `key${shortkey}`) {
        this.click();
      }
    });
  }
}
</script>

<style scoped lang="scss">
  .game-lights-button {
   // outline: none;
   // animation: effect_dylan 800ms;
    //-webkit-tap-highlight-color: transparent;   

    
     
    button {
          
        background: rgb(233, 10, 10);
        position: relative;
        float: center;
        margin-right: 3em;	
        width: 300px;
        height: 300px;
        -webkit-border-radius: 150px 150px 150px 150px;
        border-radius: 150px 150px 150px 150px;
        -moz-box-shadow: 2px 1px 12px #aaa;
        -webkit-box-shadow: 2px 1px 12px #aaa;
        -o-box-shadow: 2px 1px 12px #aaa;
        box-shadow: 2px 1px 12px #aaa;
      
      &:before {
                  height: 290px;
                  -webkit-border-radius: 150px 150px 150px 150px;
                  border-radius: 150px 150px 150px 150px;
                  position: absolute;
                  text-indent: 10000px;
        content: '';
      background-color: currentColor;
      border-radius: 50%;
      display: block;
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      transform: scale(0.001, 0.001);
      }
      &.click {
        opacity: 1;
        pointer-events: none;
        user-select: none;
        &:before {
          outline: none;
          pointer-events: none;        
          animation: effect_dylan 800ms;
        }
      }      
    }
    @keyframes effect_dylan {
      50% {
        will-change: scale;
        transform: scale(3, 3);
        opacity: 0;
      }
      99% {
        will-change: scale;
        transform: scale(0.001, 0.001);
        opacity: 0;
      }
      100% {
        will-change: scale;
        transform: scale(0.001, 0.001);
        opacity: 1;
      }
    }  
  } 
</style>
