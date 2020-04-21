<template>
  <div class="game">
    <div class="board" :style="result ? {pointerEvents: 'none'} : ''">
    <div class="cell" 
      @click="turnEfect" 
      v-for="cell in cells" 
      :key="cell" 
      :id="cell"></div>  
    <div class="result" v-show="result">{{winingText}}</div>
    </div>
    <!-- <button @click.prevent="gameStart" class="replay">Replay</button> -->
  </div>
</template>

<script>
  export default {
    data() {
      return {
        gameStarted: false,
        gameBoard: [],
        huPlayer: 'X',
        aiPlayer: 'O',
        cells: [],
        result: false,
        winingText: '',
        winCombo: [
          [0,1,2],
          [3,4,5],
          [6,7,8],
          [0,4,8],
          [2,4,6],
          [1,4,7],
          [0,3,6],
          [2,5,8],
        ]
      }
    },
    methods: {
      gameStart () {
        this.gameStarted = true;
        this.gameBoard = Array.from(Array(9).keys())
        this.cells = Array.from(Array(9).keys())
      },
      turnEfect(cell) {
        this.turn(cell);
      },
      turn(cell) {
        if(this.gameStarted) {
          this.gameBoard[cell.target.id] = this.huPlayer
          cell.target.innerHTML = this.huPlayer
          let gameWon = this.checkWin();
          if(gameWon) this.gameOver(gameWon)
        }
      },
      checkWin() {
        let plays = this.gameBoard.reduce((a,c,i) => c === this.huPlayer ? a.concat(i) : a, [])
        let gameWon = null;
        for (const [index, combo] of this.winCombo.entries()) {
          if(combo.every(item => plays.indexOf(item) > -1)){
            gameWon = {index, huPlayer: this.huPlayer};
            break;
          }
        }
        return gameWon;
      },
      gameOver() {
        this.result = true;
        this.winingText = 'You Win!'
      }
    },
    created () {
      this.gameStart();
    }
  }
</script>

<style scoped>
  .game{
    text-align: center;
  }
  .board{
    display: flex;
    flex-wrap: wrap;
    margin: 50px auto;
    text-align: center;
    width: 300px;
    position: relative;
  }
  .cell{
    max-width: 100px;
    width: 100px;
    height: 100px;
    line-height: 100px;
    border: 2px solid #000;
    font-size: 40px;
    font-weight: 700;
  }
  .cell:nth-child(1),
  .cell:nth-child(2),
  .cell:nth-child(3){
    border-top: 0px;
  }
  .cell:nth-child(1),
  .cell:nth-child(4),
  .cell:nth-child(7){
    border-left: 0px;
  }
  .cell:nth-child(3),
  .cell:nth-child(6),
  .cell:nth-child(9){
    border-right: 0px;
  }
  .cell:nth-child(7),
  .cell:nth-child(8),
  .cell:nth-child(9){
    border-bottom: 0px;
  }
  .result{
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: rgba(57, 134, 25, 0.7);
    padding: 50px;
    color: #fff;
    text-transform: uppercase;
    letter-spacing: 2px;
    font-weight: 700;
    font-size: 20px;
  }
  .replay{
    margin: 20px auto;
  }
</style>