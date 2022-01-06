<template>
  <div class="keyboard-wrapper">
    <div class="keyboard">
      <button id="newgamebtn"  v-if="isPlaying" :disabled="isBtnDisabled"></button>
      <button  id="resetbtn"   v-if="!isPlaying" @click="startNewGame" :disabled="isBtnDisabled"></button>
      <!-- <button id="simple-btn" :disabled='playing'>A Simple Button</button> -->
        
      <div class="row">
      <button @click="keyClick" value="q" id="q">Q</button>
      <button @click="keyClick" value="w" id="w">W</button>
      <button @click="keyClick" value="e" id="e">E</button>
      <button @click="keyClick" value="r" id="r">R</button>
      <button @click="keyClick" value="t" id="t">T</button>
      <button @click="keyClick" value="y" id="y">Y</button>
      <button @click="keyClick" value="u" id="u">U</button>
      <button @click="keyClick" value="i" id="i">I</button>
      <button @click="keyClick" value="o" id="o">O</button>
      <button @click="keyClick" value="p" id="p">P</button>
      </div>
      
      <!-- removing value attribute; using e.target.innerHTML -->
      <!-- instead of e.target.value -->
      <div class="row">
        <div class="spacer">&nbsp;&nbsp;</div>
        <!-- <button>H</button> -->
        <button @click="keyClick" id="a">A</button>
        <button @click="keyClick" id="s">S</button>
        <button @click="keyClick" id="d">D</button>
        <button @click="keyClick" id="f">F</button>
        <button @click="keyClick" id="g">G</button>
        <button @click="keyClick" id="h">H</button>
        <button @click="keyClick" id="j">J</button>
        <button @click="keyClick" id="k">K</button>
        <button @click="keyClick" id="l">L</button>
        <div class="spacer">&nbsp;&nbsp;</div>
        <!-- <div class="spacer"></div> -->
      </div>

      <div class="row">
        <button class="large-btn" @click="keyClick">ENTER</button>
        <button @click="keyClick" id="z">Z</button>
        <button @click="keyClick" id="x">X</button>
        <button @click="keyClick" id="c">C</button>
        <button @click="keyClick" id="v">V</button>
        <button @click="keyClick" id="b">B</button>
        <button @click="keyClick" id="n">N</button>
        <button @click="keyClick" id="m">M</button>
        <button class="large-btn" @click="keyClick">BACK</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Keyboard',
  props: {
    keyColors: Object,
    isDisabled: Boolean,
    newGame: Boolean,
    isPlaying: Boolean,
    restartImage: Boolean,
  },
  emits: ['key', 'newGameKeys', 'newGame', 'keyColors'],
  data () {
    return {
      disabled: Boolean,
      playing: true,
    }
  },
  computed: {
  isBtnDisabled() {
    // evaluate whatever you need to determine disabled here...
    return this.isPlaying;
    }
  },
  methods:
  {
    keyClick(e){
      // console.log(e.target.value.toUpperCase());
      console.log(`Passing the ${e.target.innerHTML} key up to parent..`);
      this.$emit('key', e.target.innerHTML);
    },
    startNewGame(){
      console.log('Clicked New Game!');
      this.decolorButtons();

      // function sleep(milliseconds) {
      //   const date = Date.now();
      //   let currentDate = null;
      //   do {
      //     currentDate = Date.now();
      //   } while (currentDate - date < milliseconds);
      // }
      // sleep(1000);
      // const playBtn = document.getElementbyId('newgamebtn');
      // const newBtn = document.getElementById('resetbtn');
      // playBtn.classList.remove('visible');
      // playBtn.classList.add('hidden');

      // newBtn.classList.remove('hidden');
      // newBtn.classList.add('visible');


          // setTimeout(() => {
          //   messageNode.classList.remove('visible');
          //   messageNode.classList.add('hidden');
          // }, 1500
          // )
      console.log(`Values in Keyboard after NEW GAME: isPlaying: ${this.isPlaying} new: ${this.newGame} keys:${this.keyColors[0]}`);
      // this.$emit('newGame', true); // I HAVE TO RETURN THIS
      this.$emit('newGameKeys', {'newGame': true, 'keyColors': null});


    },
    disableButtons(){
      console.log('Disabling buttons');
      let b = document.querySelectorAll('button');
      for (let i = 1; i < b.length; i++){
        b[i].disabled = true;
      }
    },
    enableButtons(){
      console.log('Enabling buttons');
      let b = document.querySelectorAll('button');
        for (let i = 1; i < b.length; i++){
          console.log('Setting default background-color');
          // b[i].style.backgroundColor = '-internal-light-dark(rgb(239, 239, 239), rgb(59, 59, 59))';
          b[i].disabled = false;
          // b[i].style.backgroundColor = 'blue';
        }
      },
      decolorButtons() {
        console.log('Resetting to default color');
        let b = document.querySelectorAll('button');
        for (let i = 1; i < b.length; i++){
          console.log('Setting default background-color');
          b[i].style.borderStyle = 'outset';
          b[i].style.backgroundColor = '#efefef';
          b[i].style.borderColor = '#858585';
          b[i].style.borderWidth = '0.5px';
          // b[i].style.backgroundColor = '-internal-light-dark(rgb(239, 239, 239), rgb(59, 59, 59))';
        }
      },
    colorButtons(){
        console.log('Coloring buttons');
        for (let k in this.keyColors) {
          document.getElementById(k).style.backgroundColor = this.keyColors[k];
        }
    },
  },
  mounted(){
    this.disable = this.isPlaying;
    this.enableButtons();
  },
  updated() {

    console.log("Keyboard updated!");
    console.log(`Values in Keyboard after updated(): isPlaying: ${this.isPlaying} new: ${this.newGame} keys:${this.keyColors}`);
    // then check if gameover

    // if won, gameboard sets isPLaying = false newGame = false

    if (!this.isPlaying && !this.newGame){
      // this.colorButtons(); 
      this.disableButtons(); 
      this.$emit('keyColors', null);
    } 
    else if (!this.isPlaying && this.newGame){
      this.enableButtons();
      this.$emit('newGame', false);
    }
    if (this.keyColors != null){
      this.colorButtons();
    }
    else{
      this.enableButtons();
    }
    if (this.isPlaying && this.newGame){
      this.enableButtons();
    }


    // if (this.newGame){
      
    // }
    // else {

    // if (this.newGame){
    //     this.enableButtons();
    //   }
    // else if (this.isPlaying) {    
    //   this.colorButtons();
    // }
  // }
    
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

  



  div.spacer {
    /*margin: .2rem;*/
    display: inline;
    width: calc(43px / 2.0);
    height: 58px;
    /*background-color: #353535;*/
    padding: 1px 6px;
  }

  .keyboard-wrapper {
    display: flex;
    justify-content: center;
  }

  #simple-button:disabled {
    cursor: not-allowed;
    opacity: 0.8;
  }


  #newgamebtn {
    margin-bottom: 1rem;
    font-family: 'Clear Sans', sans-serif;
    width: 64px;
    height: 64px;
    font-weight: 600;
    /*box-shadow: 2px 2px black;*/
    /*color: #ddd;*/
    border: 5px solid #444;
    /*outline: 5px solid #444;*/
    outline-right: 5px solid #444;
    border-radius: 50%;
    font-size: 1rem;
    background-color: #efefef;
    /*background-image: url('../assets/play.png');*/
    background-size: cover;
    /*backgroun-size: fit;*/
    text-transform: uppercase;
    /*border-radius: 1rem;*/
    position: absolute;
    top: -70px;
  }

  #newgamebtn:hover{
    cursor: pointer;
    /*transform:  scale(.98);*/
  }

 /* #newgamebtn::after {
    content: ' ';
    position: absolute;
    height: 500px;
    width: 415px;
    left: -180px;
    bottom: 25px;
    z-index: -3;
    border: 20px solid #444;
  }
*/
  #resetbtn {
    margin-bottom: 1rem;
    font-family: 'Clear Sans', sans-serif;
    width: 64px;
    height: 64px;
    font-weight: 600;
    background-color: white;
    /*box-shadow: 2px 2px black;*/
    /*color: #ddd;*/
    border: 5px solid #efefef;
    /*outline: 5px solid #444;*/
    /*outline: 5px solid #444;*/
    border-radius: 50%;
    font-size: 1rem;
    background-image: url('../assets/restart.png');
    background-size: cover;
    /*backgroun-size: fit;*/
    text-transform: uppercase;
    /*border-radius: 1rem;*/
    position: absolute;
    top: -70px;
  }

  #resetbtn:hover{
    cursor: pointer;
    /*transform:  scale(.98);*/
  }

  /*#resetbtn::after {
    content: ' ';
    position: absolute;
    height: 500px;
    width: 415px;
    left: -180px;
    bottom: 25px;
    z-index: -3;
    border: 20px solid #444;
  }*/

  .keyboard {
    margin-top: 2rem;
    margin-bottom: 5rem;
    display: flex;
    flex-direction: column;
    align-items: center;
    position: relative;
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

  button[disabled] {
    cursor: not-allowed;
  }
  button[disabled]::after {
    /*outline: 5px solid #r;*/
    cursor: not-allowed;
  }
  button {
    font-family: 'Clear Sans', sans-serif;
    margin: .2rem;
    height: 58px;
    width: 43px;
    /*border-color: #444;*/
    /*background-color: #eee;*/
    /*border-width: 3px;*/

  }

  button:hover {
    background-color: #efefef;
    border: 0.5px solid grey;
    cursor: pointer;
  }

  button.large-btn {
    width: calc(43px * 1.5);
  }

  @media (max-width: 550px) {

    button {
    font-family: 'Clear Sans', sans-serif;
    margin: .1rem;
    height: 58px;
    width: 30px;
  }

  button:hover {
    /*background-color: #b3b3b3;*/
    cursor: pointer;
  }

  button.large-btn {
    /*font-size: .5rem;*/
    width: calc(40px * 1.5);
  }

  div.spacer {
    /*margin: .2rem;*/
    display: inline;
    width: calc(30px / 2.0);
    height: 58px;
    /*background-color: #353535;*/
    padding: 1px 6px;
  }

  #newgamebtn {
    margin-bottom: 1rem;
    font-family: 'Clear Sans', sans-serif;
    width: 56px;
    height: 56px;
    top: -60px;
    }
  #resetbtn {
    width: 48px;
    height: 48px;
    top: -60px;
  }
}



</style>