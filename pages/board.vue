<template>
  <div id="board" class="board-row">
    <div class="board-row" v-for="(square_line_list, xIdx) in square_list" :key="square_line_list.id">
      <button class="square" @click="changeButtonState(xIdx, yIdx)" v-for="(item, yIdx) in square_line_list" :key="item.id">
          {{ item }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'board',
  data() {
    return {
      square_list: [],
      xIsNext: true
    }
  },
  created: function(){
    this.square_list = this.getSquareList();
  },
  methods: {
    getSquareList: function(){
        const BOARD_SIZE = 3;
        const square_list = [];

        for(let i = 0; i < BOARD_SIZE; i++) {
          const square_line_list = [];
            for(let j = 0; j < BOARD_SIZE; j++) {
                const number = (BOARD_SIZE * i) + j;
                square_line_list.push(number);
            }
            square_list.push(square_line_list);
        }
        return square_list;
    },
    changeButtonState: function(x, y){
      var vm = this;
      vm.$set(this.square_list[x], y, (this.xIsNext ? 'X' : 'O'));
      vm.xIsNext = !vm.xIsNext;
    }
  }
};
</script>