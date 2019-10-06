<template>
    <div class="board">
        <div v-for="i in 4" :id="'borad-row-' + i" class="board-row">
            <div v-for="j in 4" :id="'borad-' + i + '-' + j" :style="boardBlock" class="board-block">
                <transition>
                    <div v-if=Boolean(boardNums[i-1][j-1]) :style="numberStyle[boardNums[i-1][j-1]]" class="board-number">
                        {{boardNums[i-1][j-1]}}
                    </div>
                </transition>
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
            hasAdded: [
                [false, false, false, false],
                [false, false, false, false],
                [false, false, false, false],
                [false, false, false, false]
            ],
            numberStyle: {
                2: { backgroundColor: '#eee4da', color: '#776e65' },
                4: { backgroundColor: '#ede0c8', color: '#776e65' },
                8: { backgroundColor: '#f2b179' },
                16: { backgroundColor: '#f59563' },
                32: { backgroundColor: '#f67c5f' },
                64: { backgroundColor: '#f65e3b' },
                128: { backgroundColor: '#edcf72' },
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
        this.initGame()
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
                        this.$set(this.boardNums[i], j, Math.random() < 0.5 ? 2 : 4)
                        break
                    } else {
                        i = Math.floor(Math.random() * 4)
                        j = Math.floor(Math.random() * 4)
                    }
                }
            } else {
                return false
            }
        },
        initGame() {
            for (let i = 0; i < 4; i++) {
                for (let j = 0; j < 4; j++) {
                    this.$set(this.boardNums[i], j, 0)
                }
            }
            this.generateOneNumber()
            this.generateOneNumber()
        },
        Left() {
            if (!this.canMoveLeft()) {
                return false
            } else {
                for (let i = 0; i < 4; i++) {
                    for (let j = 1; j < 4; j++) {
                        if (this.boardNums[i][j] != 0) {
                            for (let k = 0; k < j; k++) {
                                if (this.boardNums[i][k] == 0 && this.noBlockHorizontal(i, k, j)) {
                                    //move
                                    this.$set(this.boardNums[i], k, this.boardNums[i][j])
                                    this.$set(this.boardNums[i], j, 0)
                                } else if (this.boardNums[i][k] == this.boardNums[i][j] && this.noBlockHorizontal(i, k, j) && !this.hasAdded[i][k]) {
                                    //move
                                    //add
                                    this.$set(this.boardNums[i], k, 2 * this.boardNums[i][j])
                                    this.$set(this.boardNums[i], j, 0)
                                    this.hasAdded[i][k] = true
                                }
                            }

                        }
                    }
                }
                for (let i = 0; i < 4; i++) {
                    for (let j = 0; j < 4; j++) {
                        this.hasAdded[i][j] = false
                    }
                }
                this.generateOneNumber()
                this.isGameOver()
            }
        },
        Up() {
            if (!this.canMoveUp()) {
                return false
            } else {
                for (let i = 1; i < 4; i++) {
                    for (let j = 0; j < 4; j++) {
                        if (this.boardNums[i][j] != 0) {
                            for (let k = 0; k < i; k++) {
                                if (this.boardNums[k][j] == 0 && this.noBlockVertical(j, k, i)) {
                                    //move
                                    this.$set(this.boardNums[k], j, this.boardNums[i][j])
                                    this.$set(this.boardNums[i], j, 0)
                                } else if (this.boardNums[k][j] == this.boardNums[i][j] && this.noBlockVertical(j, k, i) && !this.hasAdded[k][j]) {
                                    //move
                                    //add
                                    this.$set(this.boardNums[k], j, 2 * this.boardNums[i][j])
                                    this.$set(this.boardNums[i], j, 0)
                                    this.hasAdded[k][j] = true
                                }
                            }

                        }
                    }
                }
                for (let i = 0; i < 4; i++) {
                    for (let j = 0; j < 4; j++) {
                        this.hasAdded[i][j] = false
                    }
                }
                this.generateOneNumber()
                this.isGameOver()
            }
        },
        Right() {
            if (!this.canMoveRight()) {
                return false
            } else {
                for (let i = 0; i < 4; i++) {
                    for (let j = 2; j >= 0; j--) {
                        if (this.boardNums[i][j] != 0) {
                            for (let k = 3; k > j; k--) {
                                if (this.boardNums[i][k] == 0 && this.noBlockHorizontal(i, j, k)) {
                                    //move
                                    this.$set(this.boardNums[i], k, this.boardNums[i][j])
                                    this.$set(this.boardNums[i], j, 0)
                                } else if (this.boardNums[i][k] == this.boardNums[i][j] && this.noBlockHorizontal(i, j, k) && !this.hasAdded[i][k]) {
                                    //move
                                    //add
                                    this.$set(this.boardNums[i], k, 2 * this.boardNums[i][j])
                                    this.$set(this.boardNums[i], j, 0)
                                    this.hasAdded[i][k] = true
                                }
                            }

                        }
                    }
                }
                for (let i = 0; i < 4; i++) {
                    for (let j = 0; j < 4; j++) {
                        this.hasAdded[i][j] = false
                    }
                }
                this.generateOneNumber()
                this.isGameOver()
            }
        },
        Down() {
            if (!this.canMoveDown()) {
                return false
            } else {
                for (let i = 2; i >= 0; i--) {
                    for (let j = 0; j < 4; j++) {
                        if (this.boardNums[i][j] != 0) {
                            for (let k = 3; k > i; k--) {
                                if (this.boardNums[k][j] == 0 && this.noBlockVertical(j, i, k)) {
                                    //move
                                    this.$set(this.boardNums[k], j, this.boardNums[i][j])
                                    this.$set(this.boardNums[i], j, 0)
                                } else if (this.boardNums[k][j] == this.boardNums[i][j] && this.noBlockVertical(j, i, k) && !this.hasAdded[k][j]) {
                                    //move
                                    //add
                                    this.$set(this.boardNums[k], j, 2 * this.boardNums[i][j])
                                    this.$set(this.boardNums[i], j, 0)
                                    this.hasAdded[k][j] = true
                                }
                            }

                        }
                    }
                }
                for (let i = 0; i < 4; i++) {
                    for (let j = 0; j < 4; j++) {
                        this.hasAdded[i][j] = false
                    }
                }
                this.generateOneNumber()
                this.isGameOver()
            }
        },
        canMoveLeft() {
            for (let i = 0; i < 4; i++) {
                for (let j = 1; j < 4; j++) {
                    if (this.boardNums[i][j] != 0) {
                        if (this.boardNums[i][j - 1] == 0 || this.boardNums[i][j - 1] == this.boardNums[i][j]) {
                            return true
                        }
                    }
                }
            }
            return false
        },
        canMoveUp() {
            for (let i = 1; i < 4; i++) {
                for (let j = 0; j < 4; j++) {
                    if (this.boardNums[i][j] != 0) {
                        if (this.boardNums[i - 1][j] == 0 || this.boardNums[i - 1][j] == this.boardNums[i][j]) {
                            return true
                        }
                    }
                }
            }
            return false
        },
        canMoveRight() {
            for (let i = 0; i < 4; i++) {
                for (let j = 0; j < 3; j++) {
                    if (this.boardNums[i][j] != 0) {
                        if (this.boardNums[i][j + 1] == 0 || this.boardNums[i][j + 1] == this.boardNums[i][j]) {
                            return true
                        }
                    }
                }
            }
            return false
        },
        canMoveDown() {
            for (let i = 0; i < 3; i++) {
                for (let j = 0; j < 4; j++) {
                    if (this.boardNums[i][j] != 0) {
                        if (this.boardNums[i + 1][j] == 0 || this.boardNums[i + 1][j] == this.boardNums[i][j]) {
                            return true
                        }
                    }
                }
            }
            return false
        },
        //水平向右，col1 < col2
        noBlockHorizontal(row, col1, col2) {
            for (let i = col1 + 1; i < col2; i++) {
                if (this.boardNums[row][i] != 0) {
                    return false
                }
            }
            return true
        },
        //垂直向下，row1 < row2
        noBlockVertical(col, row1, row2) {
            for (let i = row1 + 1; i < row2; i++) {
                if (this.boardNums[i][col] != 0) {
                    return false
                }
            }
            return true
        },
        isGameOver() {
            if (!this.hasSpace() && !this.canMoveLeft() && !this.canMoveUp() && !this.canMoveRight() && !this.canMoveDown()) {
                alert("Game Over")
            } else {
                return false
            }
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
    font-size: 45px;
    font-weight: 600;
    line-height: 100px;
}

.board-number {
    border-radius: 6px;
    color: #fff;
}

.v-enter,
.v-leave-to {
    opacity: 0;
}

.v-enter-active,
.v-leave-active {
    transition: all .2s ease;
}
</style>