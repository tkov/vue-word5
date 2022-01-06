<template>
  <div class="title">
    <h1 style="">{{cont}}</h1>
  </div>
</template>

<script>
export default {
  name: 'Title',
  props: {
    cont: String,
    bColor: String,
  },
  updated(){
    document.querySelector('.title').style.backgroundColor = this.bColor;
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  *, *::before, *::after {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  .title{
    display: flex;
    justify-content: center;
    margin-top: 1rem;
    margin-bottom: 1rem;
  }

  p {
    color: red;
    font-size: 50px;
  }

  h1 {
    font-size: 60px;
    text-transform: uppercase;
    font-family: 'Clear Sans' , sans-serif;
    letter-spacing: 2px;
    font-weight: 200;
    background-color: #444;
    color: white;
    width: 275px;
    text-align: center;
    padding: 1rem;
    outline: 10px solid #444;
    border: 4px solid white;
  }
</style>