<template>
    <div id="modalSelect">
        <h3>
            Seleccione el turno que desea:
        </h3>
        

        <form method="POST" v-on:submit.prevent="handleSubmit">
            <label for="">Nombre:</label>
            <input name="customer" type="text" v-model="form.customer">
            <label for="">Fecha:</label>
            <input name="date" type="date" v-model="form.date" id="datefield" min="">
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
                today: '',
                form: {
                    date: '',
                    time: '',
                    customer: ''
                }    
            }
        },
    async mounted () {
        this.api = await axios.get('https://crud-dance-api.herokuapp.com/api/turnos')
        this.today = new Date();

        let today = new Date();
        let dd = today.getDate();
        let mm = today.getMonth() + 1; //January is 0!
        let yyyy = today.getFullYear();

        if (dd < 10) {
        dd = '0' + dd;
        }

        if (mm < 10) {
        mm = '0' + mm;
        } 
            
        today = yyyy + '-' + mm + '-' + dd;
        document.getElementById("datefield").setAttribute("min", today);
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
            
            if(repeated.length > 0){
                alert('Este turno ya esta ocupado, elija otro por favor!')
            }else{
                let turn = this.form
                axios.post('https://crud-dance-api.herokuapp.com/api/turnos', this.form).then(
                    response => {
                        
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

<style scoped>
    #modalSelect {
        background-color: rgba(3, 5, 7, 0.8);
        font-size: 20px;
        color: white;
        padding: 20px;
        border-radius: 10px;
    }

    form{
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    input {
        background-color: rgb(65, 21, 21, 0.8);
        border-radius: 4px;
        border: none;
        color: white;
        font-family: fantasy;
        margin: 10px;
        width: 200px;
        height: 30px;
        text-align: center;
        padding-top: 6px;
    }

    select {
        background-color: rgb(65, 21, 21, 0.8);
        border-radius: 4px;
        border: none;
        color: white;
        font-family: fantasy;
        margin: 10px;
        width: 200px;
        height: 36px;
        text-align: center;
        padding-top: 6px;
    }

    button{
        cursor: pointer;
        color: white;
        background-color: rgba(41, 3, 3, 0.8);
        border: none;
        border-radius: 5px;
        margin: 10px;
        box-shadow: black 3px 3px;
        height: 30px;
        width: 130px;
        padding-top: 6px;
        font-size: 15px;
        font-family: fantasy;

    }
    button:hover{
        background-color: rgb(26, 26, 26);
        box-shadow: black 0px 0px;
    }
</style>