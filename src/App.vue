<template id="app">
  <div id="app">
    <div id="header">
      <AllButtons @showModalSelect="showModalSelectFunction" @showModalDeath="showModalDeathFunction"/>
    </div>
    <div id="main-container">
      <button class="buttonFinish" v-if="isDeath" @click="finishDeleting">Terminar de eliminar</button>
      <ReservedTurns :isDeath="this.isDeath" :newTurn="this.newTurn"/>
    </div>
    <div id="footer">
      <transition name="fade">
        <div class="modal-overlay modal-lg" v-if="showModalSelect">
          <ModalSelectTurn class="modal" @closeModal="closeModalSelect" @submitTurn="submitTurn"/>    
        </div>
      </transition>
      <transition name="fade">
        <div class="modal-overlay modal-lg" v-if="showModalDeath">
          <ModalDeath class="modal" @isDeath="deathHere" @closeModal="closeModalDeath"/>    
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
      if(!confirm(`Has seleccionado el turno del ${turn.date} a las ${turn.time}. Deseas reservar un turno mas?`)){
        this.showModalSelect = false
      }
      this.newTurn = 'Nuevo turno'

    },
    clean: function(){
      this.newTurn = ''
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
  watch:{
    newTurn: function(){
      setTimeout(this.clean,1000);
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
#appHTML{
  background-image: url('https://www.fondosdepantalla.top/wp-content/uploads/2016/05/fondos-4K-HD-de-La-Muerte-Fondosdepantalla.top-1.jpg');
  -webkit-background-size: cover;
    -moz-background-size: cover;
    -o-background-size: cover;
    background-size: cover;
    min-height: 100vh;

}
#app {
  
  font-family: Avenir, Helvetica, Arial, sans-serif;
  /* -webkit-font-smoothing: antialiased; */
  /* -moz-osx-font-smoothing: grayscale; */
  text-align: center;
  /* color: #2c3e50; */
  /* margin-top: 60px; */
  font-family: fantasy;
  
  
}

.buttonFinish{
  cursor: pointer;
  color: white;
  background-color: rgb(113, 27, 27);
  border: none;
  border-radius: 5px;
  margin: 10px;
  box-shadow: black 3px 3px;
  height: 30px;
  width: 230px;
  padding-top: 6px;
  font-size: 15px;
  font-family: fantasy;
  margin-bottom: 50px;
  margin-top: -20px;
}    

.buttonFinish:hover{  
  background-color: rgb(26, 26, 26);
  box-shadow: black 0px 0px;
    
}

.modal{
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.modal-overlay{
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  z-index: 100;
  background: rgba(0, 0, 0, 0.6);
}


</style>
