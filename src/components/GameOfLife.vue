<template>
  <div class="container"> 
    <div class="row">
      <div class="col-12 mb-4">
        <h1 class="text-light">Game of life</h1>
        <button class="btn btn-success" @click="startGame">start</button>
      </div>
    </div>
    <div class="row">
      <div class="col-12 d-flex justify-content-center">
        <div class="bg-custom">
          <div v-for="r in rowsGame" :key="r" class="col d-flex">
            <div v-for="c in colsGame" :key="c" class="cube rounded-pill" :class="{ 'bg-cell-alive' : isAlive(r, c), 'bg-cell-death' : !isAlive(r, c) }" ></div> 
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'GameOfLife',
  data(){
    return{
      matrix : new Map(),
      rowsGame: 80,
      colsGame: 100
    }
  },
  mounted(){
    for (let r = 1; r <= this.rowsGame; r++) {
      for (let c = 1; c <= this.colsGame; c++) {
        let key = `${r}-${c}`;
        let value = Math.round(Math.random())
        this.matrix.set(key, value)
      }
    }
  },
  methods: {
    isAlive(r, c){
      let k = `${r}-${c}`
      return this.matrix.get(k)
    },
    controls(key){
      let r = Number(key.split('-')[0])
      let c = Number(key.split('-')[1])
      let n1 = this.matrix.has(`${r - 1}-${c - 1}`) ? this.matrix.get(`${r - 1}-${c - 1}`) : 0
      let n2 = this.matrix.has(`${r}-${c - 1}`) ? this.matrix.get(`${r}-${c - 1}`) : 0
      let n3 = this.matrix.has(`${r - 1}-${c}`) ? this.matrix.get(`${r - 1}-${c}`) : 0
      let n4 = this.matrix.has(`${r + 1}-${c - 1}`) ? this.matrix.get(`${r + 1}-${c - 1}`) : 0
      let n5 = this.matrix.has(`${r - 1}-${c + 1}`) ? this.matrix.get(`${r - 1}-${c + 1}`) : 0
      let n6 = this.matrix.has(`${r + 1}-${c + 1}`) ? this.matrix.get(`${r + 1}-${c + 1}`) : 0
      let n7 = this.matrix.has(`${r}-${c + 1}`) ? this.matrix.get(`${r}-${c + 1}`) : 0
      let n8 = this.matrix.has(`${r + 1}-${c}`) ? this.matrix.get(`${r + 1}-${c}`) : 0
      const result = n1 + n2 + n3 + n4 + n5 + n6 + n7 + n8
      return result
    },
    nextTicket(){
      let arrayMatrixCopy = Array.from(this.matrix)
      let matrixCopy = new Map(arrayMatrixCopy)

      for (let r = 1; r <= this.rowsGame; r++) {
        for (let c = 1; c <= this.colsGame; c++) {
          let key = `${r}-${c}`
          let k = this.controls(key)

          if (matrixCopy.get(key) && k < 2) {
            matrixCopy.set(key, 0)
          } 
          if (matrixCopy.get(key) && k > 3) {
            matrixCopy.set(key, 0)
          } 
          if (matrixCopy.get(key) == 0 && k == 3) {
            matrixCopy.set(key, 1)
          } 
        }
      }
      this.matrix = matrixCopy
    },
    startGame(){
      setInterval(() => {
          this.nextTicket()
      }, 300);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .cube{
    height: 30px;
    width: 30px;
  }
  .bg-custom{
    background-color: white;
  }
  .bg-cell-death{
    /* background-color: rgb(101, 99, 99); */
    background-image: url('./../../public/death.png');
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center;
  }
  .bg-cell-alive{
    /* background-color: rgb(101, 99, 99); */
    background-image: url('./../../public/alive.png');
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center;
  }
  .svg-icon {
    width: 1em;
    height: 1em;
  }

  .svg-icon path,
  .svg-icon polygon,
  .svg-icon rect {
    fill: #4691f6;
  }

  .svg-icon circle {
    stroke: #4691f6;
    stroke-width: 1;
  }
</style>
