<template>

  <section class="src-componentes-navbar">
    <div id="navigator">
      <button id="reset" @click="newColors()"> {{ textoRestartIn }}</button>
      <span id="message" v-bind:style="{ color:  (mensajeIn != '') ? '#000000' : '#ffffff' }"> {{ mensajeIn }} </span>

      <button id="easy" v-bind:class="{ selected: !isHard }" @click="setEasyMode()">easy</button>
      <button id="hard" v-bind:class="{ selected: isHard }" @click="setHardMode()">hard</button>
    </div>

  </section>

</template>

<script>

  export default  {
    name: 'src-componentes-navbar',
    props: ['mensajeIn', 'textoRestartIn'],
    components: {

    },
    mounted () {

    },
    data () {
      return {
        isHard: true,
        mensaje: null
      }
    },
    methods: {
      setEasyMode(){
        if (this.isHard) {
          this.isHard = false
          this.$emit('is-hard', this.isHard)
        }
      },
      setHardMode(){
        if (!this.isHard) {
          this.isHard = true;
          this.$emit('is-hard', this.isHard)
        }
      },
      newColors() {
        this.$emit('new-colors', true)
      }
    },
    computed: {

    }
}


</script>

<style scoped lang="css">
  #navigator {

    background: #ffffff;
    height: 30px;
    text-align: center;
    margin: 0;
    margin-top: -30px;

  }

  #message {
    color: #ffffff;
    display: inline-block;
    width: 20%;
  }

  #messageBox {
    
  }

  .selected {
    background-color: steelblue;
    color: white;
  }

  button {
    border: none;
    background-color: white;
    font-family: "Montserrat", "Avenir";
    text-transform: uppercase;
    height: 100%;
    font-weight: 700;
    letter-spacing: 1px;
    color: steelblue;
    transition: all 0.3s;
    outline: none;
  }
  button:hover {
    color: white;
    background-color: steelblue;
  }
</style>
