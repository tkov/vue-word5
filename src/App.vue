<template>
  <!-- <Title /> -->
  <GameBoard :isPlaying="play" :keyPressed="key" @keyColors="getKeyColors" @isPlaying="getIsPlaying" :tar="word" @newGame="isNewGame" :newGame="start" @playingNewGame="getPlayingNewGame" @restart="getRestart"/>
  <Keyboard :isPlaying="play" :keyColors="keyColors" @key="getKey" @newGame="isNewGame" :newGame="start" @newGameKeys="getNewGameKeys" :restartImage="restart"/>
  <!-- takes in: isPlaying Boolean, keyColors Array  -->

</template>

<script>

// import Title from './components/Title.vue'
import GameBoard from './components/GameBoard.vue'
import Keyboard from './components/Keyboard.vue'

export default {
  name: 'App',
  components: {
    GameBoard,
    Keyboard,
  },
  data () {
    return {
      key: null,
      keyColors: null,
      spaces: 1,
      word: null,
      fiveDict: null,
      play: null,
      start: null,
      restart: false,
    }
  },
  methods: {
    resetDuplicate() {
      this.spaces = 1;
    },
    getPlayingNewGame({playing, game}){
      this.play = playing;
      this.start = game;
    },
    getRestart(value){
      this.restart = value;
    },
    getNewGameKeys({newGame, keys}){
      console.log('Setting keys to null, a new game has started...')
      this.start = newGame;
      this.keyColors = keys;
    },
    getKey (value) {
      console.log(`${value} was entered...`);

      if (value === this.key){  // same key press
        this.spaces = this.spaces + 1;
        this.key = value + Array(this.spaces).join(' ');
      }
      else {  // different key press
        this.key = value;
        this.resetDuplicate();
      }
    },
    getKeyColors(value) {
       this.keyColors = value;
    },
    getIsPlaying(value) {
      this.play = value;
    },
    generateWord(){
      let len = Object.keys(this.fiveDict).length
      // console.log(len);

      function getRandomInt(max) {
        return Math.floor(Math.random() * max);
      }

      let index = getRandomInt(len);

      function getObjectValueFromIndex(obj, index) {
         return Object.keys(obj)[index];
      }

      this.word = getObjectValueFromIndex(this.fiveDict, index);
      console.log(`Shhhh.... the word is: ${this.word}`)
    },
    isNewGame(){
      // this.restart = false;
      this.keyColors = null;
      this.generateWord();
      this.start = true;
    }
    
  },
  mounted() {

    this.play = true;
    // this.start = false;

    const dictionaryJSON = require('./assets/words_dictionary.json');

    const newDict = {};

    for (let key in dictionaryJSON) {
      newDict[key] = String(key).length;
    }

    function nLetterDictionary(n) {

      let nDict = {}

      for (let key in newDict) {
        if (newDict[key] == n) {
          nDict[key] = newDict[key]
        }
      }

      return nDict;
    }

    let fiveDict = nLetterDictionary(5);
    this.fiveDict = fiveDict;

    this.generateWord();
  },
  updated() {
    
  },
    
}
</script>

<style>
  body {
    /*background-color: #353535;*/
  }
</style>
