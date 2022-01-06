<template>
  <Title :cont="cont" :bColor="bColor"/>
  <div class="board-wrapper">
    <h2 class="message hidden">Please enter a 5 letter word.</h2>
    <!-- <h2 id="warning">Please enter a 5 letter word.</h2> -->
    <div class="board">
      <div class="guess guess-1"> 
        <div class="tile"></div>
        <div class="tile"></div>
        <div class="tile"></div>
        <div class="tile"></div>
        <div class="tile"></div>
      </div>

      <div class="guess guess-2"> 
        <div class="tile"></div>
        <div class="tile"></div>
        <div class="tile"></div>
        <div class="tile"></div>
        <div class="tile"></div>
      </div>

      <div class="guess guess-3"> 
        <div class="tile"></div>
        <div class="tile"></div>
        <div class="tile"></div>
        <div class="tile"></div>
        <div class="tile"></div>
      </div>

      <div class="guess guess-4"> 
        <div class="tile"></div>
        <div class="tile"></div>
        <div class="tile"></div>
        <div class="tile"></div>
        <div class="tile"></div>
      </div>
      
      <div class="guess guess-5"> 
        <div class="tile"></div>
        <div class="tile"></div>
        <div class="tile"></div>
        <div class="tile"></div>
        <div class="tile"></div>
      </div>
      
      <div class="guess guess-6"> 
        <div class="tile"></div>
        <div class="tile"></div>
        <div class="tile"></div>
        <div class="tile"></div>
        <div class="tile"></div>
      </div>
    </div>
  </div>
</template>

<script>
import Title from './Title.vue'

export default {
  name: 'GameBoard',
  components: {
    Title,
    // Keyboard,
  },
  emits: ["restart","keyColors", "playingNewGame", "isPlaying", "newGame" ],
  props: {
    keyPressed: String,
    tar: String,
    newGame: Boolean,
    isPlaying: Boolean,
  },
  data () {
    return {
      key: null,
      cont: "Word5",
      bColor: "#444",
      count: 0,
      guess: 1,
      currentRowElement: null,
      currentRowTiles: null,
      targetWord: null,
      userWord: [],
      start: null,
      playing: false,
    }
  },
  methods: {
    handleGuess() {
        const messageNode = document.querySelector('.message');
        this.currentRowElement.appendChild(messageNode);
        console.log('Guess:' + this.guess + "." );
        if (this.count == 5){
          for (let index = 0; index < this.count; index++){
            this.userWord.push(this.currentRowTiles[index].innerHTML);
          }
          this.userWord = this.userWord.join('').toLowerCase();
          // run our checking function...
          let colors, keyColors; 
          [colors, keyColors] = this.checkWord(this.targetWord, this.userWord);
          for (let index = 0; index < this.count; index++){
            this.currentRowTiles[index].style.backgroundColor = colors[index];
          }

          this.$emit('keyColors', keyColors);

          // check if game won

          if (colors.every((element) => element == '#4BB74C')) {
            this.cont = "WIN";
            this.bColor = "#4BB74C";
            this.$emit('playingNewGame', {'isPlaying': false, 'newGame': false});
          } 
          // continue onto the game, i.e. the next rows...
          else {
            this.guess++;
            if (this.guess == 7) {
              this.cont = "LOSS";
              this.bColor = "#c80000";
              this.gameOver();
              return
            }
            this.count = 0;
            this.key = null;
            this.currentRowElement = document.querySelector('.guess-' + this.guess);
            this.currentRowTiles = this.currentRowElement.childNodes;
            this.userWord = [];
            return
          }


        }
        // handle a word entered less than 5 letters long...
        else {
          // this.currentRowElement.appendChild(messageNode);
          messageNode.classList.remove('hidden');
          messageNode.classList.add('visible');
          setTimeout(() => {
            messageNode.classList.remove('visible');
            messageNode.classList.add('hidden');
          }, 1500
          )

          // this.currentRowElement.removeChild(messageNode);
          // this.warningElementText = "Please enter a 5 letter word."
          return
        }
        
        // this.currentRowElement.removeChild(messageNode);
        this.userWord = [];
    },
    handleKeyPress() {

     if (this.key == "ENTER") {
      console.log("handling guess...")
      this.handleGuess();
     }
     else {

        if (this.count >= 0 && this.count <= 5){
          if (this.key == "BACK" && this.count > 0){
            this.count -= 1;
            this.currentRowTiles[this.count].innerHTML = '';
          }
          else if (this.key.length == 1 && this.count < 5) {
            console.log(this.key + " NOT THE BACK KEY");
            this.currentRowTiles[this.count].innerHTML = this.key;
            this.count += 1;
          }
        }
      }
      console.log(`Count is: ${this.count}`)
      console.log('Start: ' + this.start, 'Playing: ' + this.isPlaying, 'New Game: ' + this.newGame)
      // this.currkey = null;
    },
    checkWord(target, user) {
        const out = Array(5); 

        // user = user.toLowerCase();

        const targetArray = [...target];
        const userArray = [...user];
        console.log(targetArray)
        console.log(userArray)

        // check actual position first... then check if exists within it

        // CAPITAL - SAME POSITION
        // lowercase - contains letter

        // String.includes(...), Array.includes(...)

        for (const i in target) {
          if (targetArray[i] === user[i]) {
            out[i] = target[i].toUpperCase();
            userArray[i] = null
            targetArray[i] = null;
          }
        }

        // console.log(`After passing position we have ${out}`);

        for (const i in target) {
          if (userArray[i] !== null && targetArray.includes(user[i])) {
            console.log(`Match found: ${user[i]}`);
            let index = targetArray.indexOf(user[i]);
            targetArray[index] = null;
            out[i] = user[i] + '*';
          }
          else if (out[i] == null) {
            out[i] = '';
          }
        } //for

        const color = out.map((element) => {
          if (element.length == 0) {
            return 'grey'
          }
          else if (element.length == 1) {
            return '#4BB74C'
          }
          else {
            return 'gold'
          }
        });

        const keyColor = {};
        for (let j = 0; j < user.length; j++){
          if(!keyColor[user[j]]){
            keyColor[user[j]] = color[j];
          } 
        }
        console.log(keyColor);
        return [color, keyColor];
      }, //checkWord()
      gameOver() {
        this.$emit('playingNewGame', {'isPlaying': false, 'newGame': false});

      },
      countReset() {
        this.count = 0;
      },
  },
  mounted() {
    const currentRowElement = document.querySelector('.guess-' + this.guess);
    this.currentRowElement = currentRowElement;
    this.currentRowTiles = currentRowElement.childNodes;
    this.start = false;
    this.playing = true;
    
  },
  updated() {
      console.log("Gameboard updated!");
      // console.log(`Values in Gameboard after updated(): isPlaying: ${this.isPlaying} newGame: ${this.newGame} keys:${this.keyColors}`);
      if (this.targetWord == null && this.tar){
        this.targetWord = this.tar;
        console.log(`Shhh... the game board got the word from App: ${this.targetWord}`);
      }
      // console.log(this.keyPressed);

      // if we're playing... handle key presses...
      if(this.isPlaying){
        this.key = String(this.keyPressed).trimEnd();
        if (this.count >= 0 || this.count < 5){
          this.handleKeyPress();
        }
      }

      if (this.newGame && !this.isPlaying) {

        let tiles = document.querySelectorAll('.tile');
        for (let element of tiles){
          element.style.backgroundColor = '#efefef';
          element.innerHTML = '';
        }

        this.cont = "word5"
        this.bColor = "white"
        this.targetWord = null;
        this.count = 0;
        this.guess = 1;
        const currentRowElement = document.querySelector('.guess-' + this.guess);
        this.currentRowElement = currentRowElement;
        this.currentRowTiles = currentRowElement.childNodes;
        this.start = false;
        this.playing = true;
        this.$emit('isPlaying', true);
      }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  *, *::before, *::after {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
  }

  h2.message {
    position: absolute;
    color:  white;
    /*color:  #353535;*/
    background-color: #444;
    border-radius: 8px;
    padding: .75rem;
    text-transform: uppercase;
    font-weight: 400;
    font-size: 1em;
    font-family: 'Clear Sans', sans-serif;
    /*margin-bottom: 1rem;*/
    /*margin-top: 5px;*/
    /*top: 10px;*/
    top: 10px;
    left: 30px;
    /*margin: auto;*/
  }

  .tile {
    width: 64px;
    height: 64px;
    border: 4px solid #444;
    background-color: #efefef;
    /*display: inline-block;*/
    display: flex;
    justify-content: center;
    align-items: center;
    font-family: 'Clear Sans', sans-serif;
    font-size:  44px;
    text-align: center;
  }

  .board {
    display: flex;
    flex-direction: column;
    gap: .5rem;
    border: solid 20px #efefef;
    padding: 20px;

  }

  .guess {
    display: flex;
    gap: .5rem;
    position: relative;
  }

  .board-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 2rem;

    /*border: solid 2px black;*/
  }

  .visible {
    visibility: visible;
    opacity: 1;
    transition: opacity 2s linear;
  }

  .hidden {
    visibility: hidden;
    opacity: 0;
    transition: visibility 0s 2s, opacity 2s linear;
  }


  @media (max-width: 550px) {


    .tile {
      width: 48px;
      height: 48px;
      font-size:  30px;
      background-color: #efefef;
    }
     .board {
    display: flex;
    flex-direction: column;
    gap: .5rem;
    border: solid 10px #efefef;
    padding: 20px;
    margin: auto;

  }

  }
</style>