<template>
    <div class="board">
        <div v-for="i in 4" :id="'borad-row-' + i" class="board-row">
            <div v-for="j in 4" :id="'borad-' + i + '-' + j" :style="boardBlock" class="board-block">
                <div v-show=Boolean(boardNums[i-1][j-1]) :style="numberStyle[boardNums[i-1][j-1]]" class="board-number">
                    {{boardNums[i-1][j-1]}}
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    name: 'GameBoard',
    data() {
        return {
            deviceWidth: window.screen.availWidth,
            boardBlock: {
                width: '100px',
                height: '100px',
                borderRadius: '6px'
            },
            boardNums: [
                [0, 0, 0, 0],
                [0, 0, 0, 0],
                [0, 0, 0, 0],
                [0, 0, 0, 0]
            ],
            numberStyle: {
                2: { backgroundColor: '#eee4da', color: '#776e65' },
                4: { backgroundColor: '#ede0c8', color: '#776e65' },
                8: { backgroundColor: '#f2b179' },
                16: { backgroundColor: '#f59563' },
                32: { backgroundColor: '#f67c5f' },
                64: { backgroundColor: '#f65e3b' },
                128: { backgroundColor: '##edcf72' },
                256: { backgroundColor: '#edcc61' },
                512: { backgroundColor: '#9c0' },
                1024: { backgroundColor: '#33b5e5' },
                2048: { backgroundColor: '#09c' },
                4096: { backgroundColor: '#a6c' },
                8192: { backgroundColor: '#93c' }
            }
        }
    },
    created() {
        this.generateOneNumber()
        this.generateOneNumber()
    },
    methods: {
        hasSpace() {
            for (let i = 0; i < 4; i++) {
                for (let j = 0; j < 4; j++) {
                    if (this.boardNums[i][j] == 0) {
                        return true
                    }
                }
            }
            return false
        },
        generateOneNumber() {
            if (this.hasSpace()) {
                let i = Math.floor(Math.random() * 4)
                let j = Math.floor(Math.random() * 4)
                while (true) {
                    if (this.boardNums[i][j] == 0) {
                        this.boardNums[i][j] = Math.random() < 0.5 ? 2 : 4
                        break
                    } else {
                        i = Math.floor(Math.random() * 4)
                        j = Math.floor(Math.random() * 4)
                    }
                }
            } else return false
        }
    }
}
</script>
<style>
.board {
    width: 500px;
    height: 500px;
    border-radius: 10px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
    background-color: #bbada0;
}

.board-row {
    display: flex;
    justify-content: space-evenly;
}

.board-block {
    background-color: #ccc0b3;
    text-align: center;
    font-size: 40px;
    line-height: 100px;
}

.board-number {
    border-radius: 6px;
    color: #fff;
}
</style>