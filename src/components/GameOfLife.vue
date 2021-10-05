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
            <div v-for="c in colsGame" :key="c" class="cube border border-dark" :class="{ 'bg-cell' : isAlive(r, c) }" ></div> 
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
      rowsGame: 60,
      colsGame: 25
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
    height: 15px;
    width: 15px;
  }
  .bg-custom{
    background-image: url('./../../public/glass.png');
    background-position: center;
    background-repeat: repeat;
    background-size: cover;
  }
  .bg-cell{
    background-color: rgb(101, 99, 99);
  }
</style>
