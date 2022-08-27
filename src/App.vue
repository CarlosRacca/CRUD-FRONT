<template>
  <div id="app">
    <h1 id="title">DANZA DE LA MUERTE</h1>
  
    <div id="header">
      <AllButtons @showModalSelect="showModalSelectFunction" @showModalDeath="showModalDeathFunction"/>
    </div>
    <div id="main-container">
      <button v-if="isDeath" @click="finishDeleting">Terminar de eliminar</button>
      <ReservedTurns :isDeath="this.isDeath" :newTurn="this.newTurn"/>
    </div>
    <div id="footer">
      <transition name="fade">
        <div class="modal-overlay" v-if="showModalSelect">
          <ModalSelectTurn @closeModal="closeModalSelect" @submitTurn="submitTurn"/>    
        </div>
      </transition>
      <transition name="fade">
        <div class="modal-overlay" v-if="showModalDeath">
          <ModalDeath @isDeath="deathHere" @closeModal="closeModalDeath"/>    
        </div>
      </transition>
    </div>
    <!--<div id="app2">
      <img alt="Vue logo" src="./assets/logo.png">
      <HelloWorld msg="Welcome to Your Vue.js App"/>
    </div>-->
  </div>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import AllButtons from './components/AllButtons.vue'
import ReservedTurns from './components/ReservedTurns.vue'
import ModalSelectTurn from './components/ModalSelectTurn.vue'
import ModalDeath from './components/ModalDeath.vue'

export default {
  name: 'App',
  data () {
    return {
      newTurn: '',
      isDeath: false,
      showModalDeath: false,
      showModalSelect: false
    }
  },
  methods:{
    submitTurn: function(turn){
      alert(`Has seleccionado el turno del ${turn.date} a las ${turn.time}`)
      this.showModalSelect = false
      this.newTurn = 'Nuevo turno'

    },
    finishDeleting:function(){
      this.isDeath = false
    },
    deathHere:function(){
      this.isDeath = true
    },
    closeModalSelect: function(){
      this.showModalSelect = false
    },
    closeModalDeath: function (){
      this.showModalDeath = false
    },
    showModalSelectFunction: function(){
      this.showModalSelect = true
    },
    showModalDeathFunction: function(){
      this.showModalDeath = true
    }
  },
  components: {
    //HelloWorld,
    AllButtons,
    ReservedTurns,
    ModalSelectTurn,
    ModalDeath
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#title {
  background-color: darksalmon;
}
</style>
