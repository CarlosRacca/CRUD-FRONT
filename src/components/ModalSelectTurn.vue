<template>
    <div id="modalSelect">
        <br>
        Seleccione el turno que desea:
        <br>
        <br>
        <form method="POST" v-on:submit.prevent="handleSubmit">
            <label for="">Nombre:</label>
            <input name="customer" type="text" v-model="form.customer">
            <label for="">Fecha:</label>
            <input name="date" type="date" v-model="form.date">
            <label for="">Hora:</label>
            <select name="time" type="time" v-model="form.time">
                <option value="08:00 hs">08:00 hs</option>
                <option value="09:00 hs">09:00 hs</option>
                <option value="10:00 hs">10:00 hs</option>
                <option value="11:00 hs">11:00 hs</option>
                <option value="12:00 hs">12:00 hs</option>
                <option value="13:00 hs">13:00 hs</option>
                <option value="14:00 hs">14:00 hs</option>
                <option value="15:00 hs">15:00 hs</option>
                <option value="16:00 hs">16:00 hs</option>
                <option value="17:00 hs">17:00 hs</option>
                <option value="18:00 hs">18:00 hs</option>
                <option value="19:00 hs">19:00 hs</option>
                <option value="20:00 hs">20:00 hs</option>
            </select>
            <button>Seleccionar</button>
        </form>
        <button @click="closeModal">Cerrar</button>
        <br>
        
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'ModalSelectTurns',
    props: {
        modalOpen: Boolean,
    },

    data() {
            return {
                api: [],
                form: {
                    date: '',
                    time: '',
                    customer: ''
                }    
            }
        },
    async mounted () {
        this.api = await axios.get('https://crud-dance-api.herokuapp.com/api/turnos')
        
    },
    watch:{
        'form.date': function (){
            let day = new Date(this.form.date).getUTCDay();

            if([6,0].includes(day)){
                this.form.date = '';
                alert('La muerte solo ofrece sus danzas de lunes a viernes.');
            }
        },
        
    },
    methods:{
        handleSubmit: function(){
            let repeated = this.api.data.filter(el => el.date === this.form.date && el.time === this.form.time)
            console.log(repeated)
            if(repeated.length > 0){
                alert('Este turno ya esta ocupado, elija otro por favor!')
            }else{
                let turn = this.form
                axios.post('https://crud-dance-api.herokuapp.com/api/turnos', this.form).then(
                    response => {
                        console.log(response)
                        if(response.statusText === "Created"){
                            this.$emit('submitTurn', turn)

                        }else{
                            alert('No se ha podido seleccionar este turno')
                        }
                    }
                )
            }

        },

        closeModal: function(){
            this.$emit('closeModal')
        }
    }   
}
</script>

<style>
    #modalSelect {
        background-color: cornflowerblue;
    }
</style>