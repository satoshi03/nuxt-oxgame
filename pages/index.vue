<template>
  <section class="container">
    <div>
      <div>
      <h1 class="game-message">
        {{ message }}
      </h1>
      </div>
      <div class="panel">
        <div
          v-for="(row, rindex) in panel"
        >
          <div class="box"
            v-for="(column, cindex) in row"
            :key="column.id"
            @click="touch(rindex, cindex)"
          >
           {{ getItem(column) }}
          </div>
        </div>
      </div>
    <div class="links">
      <a
        target="_blank"
        @click="reset"
        class="button--green">リセット</a>
    </div>
    </div>
  </section>
</template>

<script>
export default {
  data() {
    const players = ["プレイヤーA", "プレイヤーB"]
    return {
      panel: [
        [0, 0, 0],
        [0, 0, 0],
        [0, 0, 0],
      ],
      players: players,
      turn: players[0],
      message: players[0] + "のターン",
      win: "",
      turnCount: 1,
    }
  },
  computed: {
  },
  methods: {
    getItem(item) {
      if(item == 0) {
        return "  "
      } else if(item == 1){
        return "○ "
      } else if(item == 2){
        return "×"
      }
    },
    touch: function(row, column) {
      if(this.isPuttable(row, column)){
        if(this.turn == this.players[0]) {
          this.panel[row].splice(column, 1, 1)
        } else {
          this.panel[row].splice(column, 1, 2)
        }
        this.next()
      }
    },
    isPuttable: function(row, column){
      return this.win == "" && this.panel[row][column] == 0
    },
    next: function() {
      if(this.checkWin()) {
        this.message = this.turn + "の勝利!!"
        this.win = this.turn
        return
      }
      if(this.checkDraw()) {
        this.message = "引き分け"
        this.win = "Draw"
        return
      }
      if(this.turn == this.players[0]) {
        this.turn = this.players[1]
      } else {
        this.turn = this.players[0]
      }
      this.message = this.turn + "のターン"
      this.turnCount += 1
    },
    checkWin: function(){
      var checkNum = 0
      if(this.turn == this.players[0])  {
        checkNum = 1
      } else {
        checkNum = 2
      }

      // 縦方向の勝利判定
      var counter = 0
      for(var i = 0; i < this.panel.length; i++) {
        for(var j = 0; j < this.panel[0].length; j++) {
          if(this.panel[i][j] != checkNum) {
            break
          }
          counter += 1
          if(counter > 2) {
            return true
          }
        }
        counter = 0
      }

      // 横方向の勝利判定
      counter = 0
      for(var j = 0; j < this.panel[0].length; j++) {
        for(var i = 0; i < this.panel.length; i++) {
          if(this.panel[i][j] != checkNum) {
            break
          }
          counter += 1
          if(counter > 2) {
            return true
          }
        }
        counter = 0
      }

      // 斜め方向の勝利判定
      if (this.panel[0][0] == checkNum &&
        this.panel[1][1] == checkNum &&
        this.panel[2][2] == checkNum) {
        return true
      }
      if (this.panel[0][2] == checkNum &&
        this.panel[1][1] == checkNum &&
        this.panel[2][0] == checkNum) {
        return true
      }

      return false
    },
    checkDraw: function() {
      return this.turnCount == this.panel.length * this.panel[0].length
    },
    reset: function() {
      this.panel = [[0, 0, 0],[0, 0, 0],[0, 0, 0]]
      this.turn = this.players[0]
      this.message = this.players[0] + "のターン"
      this.win = ""
      this.turnCount = 1
    }
  }
}
</script>

<style lang="less">
@boxsize: 80px;
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.game-message {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 22px;
  color: #35495e;
  letter-spacing: 1px;
  align-items: center;
}

.panel {
  display: flex;
  justify-content: center;
  align-items: center;
  border:1px solid black;
  margin: 10px;
  width: @boxsize * 3 + 2;
}

.box {
  display: flex;
  height: @boxsize;
  width: @boxsize;
  border:1px solid black;
  justify-content: center;
  align-items: center;
  font-size: 40px;
}
</style>
