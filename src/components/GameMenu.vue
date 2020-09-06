<template>
    <div class="game-menu">
        <div class="playing-menu" v-if="started">
        </div>
        <div class="start-menu" v-else>
            <button @click="start">Start</button>
        </div>
        
    </div>
</template>

<script>


export default {
   
    name: 'game-menu',
    components: {
    },
    data () {
        return {
            started: false,
            picked: '',
            
        }
    },
    
    methods: {
        start () {
            setTimeout(() => {
                this.restartGame();
            }, 500);
        },
        restartGame () {
            if (!this.$store.state.started) {
                this.started = true;
                window.$gamelights.restart();
            }
        },
    },
    mounted () {
        document.addEventListener('keyup', e => {
            if (e.key.toLowerCase() == 'enter' || e.code.toLowerCase() == 'enter') {
                this.restartGame();
            }
        });
    }
}
</script>

<style lang="scss" scoped>
.game-menu {
    background-color: #ffffff;
    border-radius: 8px;
    min-height: 80px;
    .playing-menu {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 8px;
        height: 80px;
        flex-wrap: wrap;
    }
    .start-menu {
        display: flex;
        align-items: center;
        justify-content: center;
        min-height: 80px;
        height: 100%;
        button {
                width: 5em;
                box-sizing: border-box;
                font-size: 1.4em;
                -webkit-border-radius: 10px 10px 10px 10px;
                border-radius: 10px 10px 10px 10px;
                background: #6DABE8;
                border: none;
                padding: 0.3em 0.6em;
            &:hover {
                background-color: #ffa500;
            }
        }
    }
}
</style>