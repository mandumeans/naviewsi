<template>
    <div id="app" class="game">
        <div class="game-board">
          <board/>
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
    }
  },
  created: function(){
  },
  methods: {
  }
}
</script>

<style>
</style>
