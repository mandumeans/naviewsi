<template>
  <div id="board" class="board-row">
    <div class="board-row" v-for="(square_line_list, xIdx) in square_list" :key="square_line_list.id">
      <button class="square" @click="changeButtonState($event, xIdx, yIdx)" v-for="(item, yIdx) in square_line_list" :key="item.id">
          {{ item.value }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'board',
  props: ['square_list', 'changeButtonState']
  ,
  data() {
  },
  created: function(){
  },
  methods: {
  }
};
</script>