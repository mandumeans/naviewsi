<template>
    <div id="app" class="game">
        <div class="game-board">
          <board :squareList="squareList" :changeButtonState="changeButtonState" :winner="winner"/>
        </div>
        <div class="game-info">
            <div >
              {{ statusMessage }}
            </div>
            <ol>
              <li v-for="history in historyMessage" :key="history.id">
                  <button @click="goToHistory(history.nowStepNumber)">{{ history.desc }}</button>
              </li>
            </ol>
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
  data: function(){
    return {
      squareList: [],
      xIsNext: true,
      winner : null,      
      history: [{
          nowSquareList: [Array(3).fill(""),Array(3).fill(""),Array(3).fill("")],
          nowStepNumber: 0,
          desc: "",
          winner: null
      }]
    }
  },
  computed: {
    // 계산된 getter
    statusMessage: function () {
      let status;
      if (this.winner) {
          if(this.winner.isDraw){
              status = 'This game is draw!!'
          } else {
              status = 'Winner: ' + this.winner.winner;
          }
      } else {
        status = 'Next player: ' + (this.xIsNext ? 'X' : 'O');
      }
      return status;
    },    
    historyMessage : function() {
      return this.history.map((step, move) => {
            const desc = move ? 
            'Go to move #(' + step.nowStepNumber % 3 + ',' + Math.floor((step.nowStepNumber) / 3) + ')' + step.nowStepNumber :
            'Go to game start';

            //const selectedChecker = (this.stepNumber === move) ?  'selected-history' : '';
            step.desc = desc;
            return step;
        });
    }
  },
  created: function(){
    this.squareList = this.getSquareList();
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
          if(squares[i][0] == "" || squares[i][1] == "" || squares[i][2] == "") {
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
        const squareList = [];

        for(let i = 0; i < BOARD_SIZE; i++) {
          const squareLineList = [];
            for(let j = 0; j < BOARD_SIZE; j++) {
                const number = (BOARD_SIZE * i) + j;
                squareLineList.push("");
            }
            squareList.push(squareLineList);
        }
        return squareList;
    },

    changeButtonState: function(event, x, y){
      var vm = this;
      if (vm.squareList[x][y] != "" || vm.winner) {
        return;
      }

      //Arary에 요소 바뀌는건 set함수를 써야 다시 렌더링됨
      vm.$set(vm.squareList[x], y, (vm.xIsNext ? 'X' : 'O'))
      vm.xIsNext = !vm.xIsNext;

      //히스토리에 현재 정보 저장

      let historyList = [];
      for(const squareLineList of vm.squareList){
        historyList.push(squareLineList.slice());
      }


      //승자 확인
      var winner = vm.calculateWinner(vm.squareList);
      if(winner != null){
        vm.winner = winner;
      }
      vm.history.push({
          nowSquareList: historyList,
          nowStepNumber: vm.history.length,
          winner: winner
      });
    },

    goToHistory: function(step){
      this.squareList = this.history[step].nowSquareList;
      this.winner = this.history[step].winner;
      this.xIsNext = (step % 2) === 0
    }

  }
}
</script>

<style>
</style>
