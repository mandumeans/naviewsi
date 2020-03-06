<template>
  <div id="board" class="board-row">
    <div class="board-row" v-for="(squareLineList, xIdx) in squareList" :key="squareLineList.id">
      <button :class="['square', winnerClass(xIdx, yIdx)]" @click="changeButtonState($event, xIdx, yIdx)" v-for="(item, yIdx) in squareLineList" :key="item.id">
          {{ item }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'board',
  props: ['squareList', 'changeButtonState', 'winner'],
  data: function() {
    return {};
  },
  computed: {
  },
  created: function(){
  },
  methods: {
    winnerClass: function(xIdx, yIdx){
      let winnerClass = "";
      if(this.winner == null || this.winner.isDraw){
        return "";
      } else {
        for(let item of this.winner.winnerLine){
          if(item[0] == xIdx && item[1] == yIdx){
            winnerClass = "squre-winner";
          }
        }
      }
      return winnerClass;
    }
  }
};
</script>