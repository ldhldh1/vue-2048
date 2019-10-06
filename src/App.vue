<template>
    <div id="app">
        <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
        <div class="header">
            <h1>2048</h1>
            <button @click="newGame">New Game</button>
            <p>score:{{score}}</p>
        </div>
        <game-board ref="gameboard"></game-board>
    </div>
</template>
<script>
// import Header2048 from "./components/Header2048.vue";
import GameBoard from "./components/GameBoard.vue"

export default {
    name: "app",
    components: {
        GameBoard
    },
    data() {
        return {
            score: 0
        }
    },
    methods: {
        newGame() {
            this.$refs.gameboard.initGame()
        },
        moveLeft() {
            this.$refs.gameboard.Left()
        },
        moveUp() {
            this.$refs.gameboard.Up()
        },
        moveRight() {
            this.$refs.gameboard.Right()
        },
        moveDown() {
            this.$refs.gameboard.Down()
        },
    },
    mounted() {
        let _this = this
        document.onkeydown = function(event) {
            let e = event || window.event || arguments.callee.caller.arguments[0]
            if (e && e.keyCode == 37) {
                // left
                _this.moveLeft()
            } else if (e && e.keyCode == 38) {
                //up
                _this.moveUp()
            } else if (e && e.keyCode == 39) {
                //righu
                _this.moveRight()
            } else if (e && e.keyCode == 40) {
                //down
                _this.moveDown()
            }
        }
    }
}
</script>
<style>
#app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}

.header {
    font-size: 25px;
    font-family: 'Arial';
}

button {
    font-size: 25px;
    color: #fff;
    background-color: #8f7a66;
    line-height: 30px;
    border-radius: 6px;
}

button:hover {
    background-color: #9f8b77;
}
</style>