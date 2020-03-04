<template>
    <div id="app" class="game">
        <div class="game-board">
          <board :square_list="square_list" :changeButtonState="changeButtonState"/>
        </div>
        <div class="game-info">
            <div>status</div>
            <ol>moves</ol>
        </div>
    </div>
</template>

<script>
import board from './board'
import 'static/board.css'

export default {
  name: 'app',
  components: {
    'board': board
  },
  data(){
    return {
      square_list: [],
      xIsNext: true
    }
  },
  created: function(){
    this.square_list = this.getSquareList();
  },
  methods: {
    calculateWinner: function(squares){
      const lines = [
          [[0,0], [0,1], [0,2]],
          [[1,0], [1,1], [1,2]],
          [[2,0], [2,1], [2,2]],
          [[0,0], [1,0], [2,0]],
          [[0,1], [1,1], [2,1]],
          [[0,2], [1,2], [3,1]],
          [[0,0], [1,1], [2,2]],
          [[0,2], [1,1], [2,0]],
      ];
      let result = null;
      
      for (let i = 0; i < lines.length; i++){
        const [ax, ay] = lines[i][0];
        const [bx, by] = lines[i][1];
        const [cx, cy] = lines[i][2];
        if(squares[ax][ay] && squares[ax][ay] === squares[bx][by] && squares[ax][ay] === squares[cx][cy]) {
            result = {
                winnerLine: lines[i],
                winner:squares[ax][ay],
                isDraw:false
            };
        }
      }

      let isEmpty = false;
      for(let i = 0; i < squares.length; i++){
          if(squares[i][0].value == "" || squares[i][1].value == "" || squares[i][2].value == "") {
              isEmpty = true;
          }
      }

      //squares가 끝났는데 승패가 안갈렸으면 
      if(isEmpty === false && result === null){
          result = {
              isDraw: true
          }
      }

      return result;
    },
    
    getSquareList: function(){
        const BOARD_SIZE = 3;
        const square_list = [];

        for(let i = 0; i < BOARD_SIZE; i++) {
          const square_line_list = [];
            for(let j = 0; j < BOARD_SIZE; j++) {
                const number = (BOARD_SIZE * i) + j;
                square_line_list.push({
                  number : number,
                  value : ""
                });
            }
            square_list.push(square_line_list);
        }
        return square_list;
    },

    changeButtonState: function(event, x, y){
      if (event) event.preventDefault();
      if (this.calculateWinner(this.square_list) || this.square_list[x][y].value != "") {
        return;
      }
      var vm = this;
      this.square_list[x][y].value = (this.xIsNext ? 'X' : 'O');
      vm.xIsNext = !vm.xIsNext;
    }
  }
}
</script>

<style>
</style>
