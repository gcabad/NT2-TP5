<template>

  <section class="src-componentes-principal">
    <div id="header"  v-bind:style="header">
    <h1>The Great <br>
      <span id="colorDisplay">{{ pickedColor }}</span>
      <br>
      Guessing Game</h1>
    </div>
    <Navbar @is-hard="isHard=$event" @new-colors="newColors=$event" :mensaje-in="mensaje" :texto-restart-in="textoBtnRestart"/>
    <div class="container" v-for="index in this.colorCount" :key=index>
      <Square :color-in="colors[index-1]" :square-number="index-1" @color-out="comprobarColor($event)"/>
    </div>
  </section>
</template>

<script>
  import Navbar from './Navbar.vue'
  import Square from './Square.vue'
  
  export default  {
    name: 'src-componentes-principal',
    props: [],
    components: {
      Navbar,
      Square
    },
    beforeMount() {
      this.restart()
    },
    mounted () {
    },
    data () {
      return {
        colors: [],
        colorCount: 6,
        isHard: true,
        mensaje: null,
        pickedColor: null,
        newColors: false,
        header: {
          backgroundColor: null,
        },
        textoBtnRestart: '',
      }
    },
    methods: {
      restart() {
        this.newColors = false
        this.colors = this.createNewColors(this.colorCount);
        this.mensaje = "Pick New Colors!";
        this.pickedColor = this.colors[this.pickColor()];
        this.header.backgroundColor = "steelblue";
        this.mensaje = "";
        this.textoBtnRestart = "New Colors!";
      },
      createNewColors(){
        var nuevosColores = []
        for (var i = 0; i < this.colorCount; i++) {
          nuevosColores.push(this.createRandomStringColor());
        }
        return nuevosColores
      },
      createRandomStringColor(){
        var newColor = "rgb(" + this.randomInt() + ", " + this.randomInt() + ", " + this.randomInt() + ")" ;
        return newColor;
      },
      randomInt(){
        return Math.floor(Math.random() * 256);
      },      
      pickColor(){
        var quantity;
        if (this.isHard) {
          quantity = 6;
        } else {
          quantity = 3;
        }
        return Math.floor(Math.random() * quantity );
      },
      comprobarColor(cuadrado) {
        if(cuadrado.color === this.pickedColor){
          this.mensaje = "You Picked Right!";
          this.setAllColorsTo(cuadrado.color);
          this.textoBtnRestart = "Play Again!";
          this.header.backgroundColor = cuadrado.color;
        } else {
          // Si hago:
          // this.colors[cuadrado.numero] = '#232323'
          // solo funciona la primera vez. Por que?
          let nuevosColors = {...this.colors}
          nuevosColors[cuadrado.numero] = "#232323";
          this.colors = nuevosColors
          this.mensaje = "Try Again!"; 
        }
      },
      setAllColorsTo(colorAPintar) {
        var nuevosColores = []
        for (var i = 0; i < this.colorCount; i++) {
          nuevosColores.push(colorAPintar);
        }
        this.colors = nuevosColores
      }
    },
    computed: {

    },
    watch: {
      isHard: function () {
        //cada vez que cambia la dificultad, llama a restart
        if(this.isHard) {
          this.colorCount = 6
        } else {
          this.colorCount = 3
        }
        this.restart()
      },
      newColors: function() {
        //cada vez que cambia el valor de newColors, llama a restart
        if(this.newColors) {
          this.restart()
        }
      }
    }
}


</script>

<style scoped lang="css">
  h1 {
    font-weight: normal;
    line-height: 1.1;
    padding: 20px 0;

  }
  #header {
    transition: all 0.3s;
    background: steelblue; 
    text-transform: uppercase;
    text-align: center;
    margin: 0;
    color: white;
    
  }
  .container {
    margin: 20px auto;
    max-width: 600px;
  }
  #colorDisplay {
    font-size: 200%;
  }

</style>