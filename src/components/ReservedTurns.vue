<template>
    <div id="availableTurns">
        <h3 class="titleTurns">Los turnos ya reservados para bailar con la muerte son:</h3>
        <table class="table1">
            <thead class="tableHead">
                <th v-for="(element) in this.turnsGrouped"
                    :key="element.id">
                        <tr>
                            {{element.date}}
                        </tr>    
                    </th>
            </thead>
            <tbody class="tableBody">
                <td  v-for="(element) in this.turnsGrouped"
                    :key="element.id">
                    <tr v-for="(el) in element.turns"
                    :key="el.id">
                        <div>{{el.time}}: {{el.customer}}</div>
                        <button :name="el.customer" :id="el.id" v-if="isDeath" @click="handleDelete">Eliminar</button>
                    </tr>
                </td>
            </tbody>
        </table>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'AvailableTurns',
    props: {
        newTurn: String,
        isDeath: Boolean
    },
    data() {
            return {
                api: [],
                turns: [],
                allDates: [],
                turnsGrouped: [],
                deletedTurn: ''
                
            }
        },
    async mounted () {
        this.api = await axios.get('https://crud-dance-api.herokuapp.com/api/turnos')
        
    },
    watch:{
        api: function (){
            this.turns = this.api.data

            if(this.allDates.length > 0){
                this.allDates = []
            }

            for(let i = 0; i < this.turns.length; i++ ){
                if(i !== this.turns.length -1){
                    if(this.turns[i].date !== this.turns[i + 1].date){
                        let date = this.turns[i].date
                        this.allDates.push(date)
                    }   
                }
            }
        },

        deletedTurn: async function(){
            if(this.deletedTurn !== ''){
                this.turns = []
                this.api = await axios.get('https://crud-dance-api.herokuapp.com/api/turnos')
            }
            this.deletedTurn = ''
        },

        newTurn: async function(){
            this.turns = []
            if(this.newTurn !== ''){
                this.api = await axios.get('https://crud-dance-api.herokuapp.com/api/turnos')
            }
        },

        allDates: function(){
            if(this.turnsGrouped.length > 0){
                this.turnsGrouped = []
            }
            this.allDates.forEach(el => {
                let date = el
                let turnsInTheSameDate = this.turns.filter(element => element.date === el)

                el = {
                    date: date,
                    turns: turnsInTheSameDate
                }

                this.turnsGrouped.push(el)
            })

            this.turnsGrouped.forEach(el=>{
                el.turns.sort(this.sortArrayTimes)
            })
        }
    },
    methods:{
        handleDelete: function(e){
            e.preventDefault()
            if(confirm(`Esta segura que desea eliminar el turno de ${e.target.name}?`)){    
                axios.delete('https://crud-dance-api.herokuapp.com/api/turnos/' + e.target.id)
                alert('Has eliminado el turno de ' + e.target.name)
                this.deletedTurn = 'Turno eliminado'
            }
        },

        sortArrayTimes: function(x, y){
            if (x.time < y.time) {return -1;}
            if (x.time > y.time) {return 1;}

            return 0;
        }

    }   
}
</script>

<style scoped>
    #availableTurns {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-around;
        text-align: left;
        margin: 10px;
        padding-bottom: 10px;
    }
    td {
        color: black;
        text-align: left;
        background-color: rgb(208, 205, 205, 0.4);
        border: solid black 2px;
        padding: 5px;
    }
    th {
        background-color: rgb(208, 205, 205, 0.4);
        border: solid black 2px;
        align-items: center;
        padding: 5px;
    }
    tr {
        display: flex;
        justify-content: space-between;
    }

    button{
        color: white;
        background-color: rgb(113, 27, 27);
        border: none;
        border-radius: 5px;
        margin: 4px;
        margin-top: 0px;
        box-shadow: black 2px 1px;
        cursor: pointer;

    }
    button:hover{
        background-color: rgb(26, 26, 26);
        box-shadow: black 0px 0px;
    }

    h3{
        margin-top: -40px;
        color: black;
        text-shadow: brown 1px 1px;
        font-size: 30px;
        /* background-color: rgb(252, 252, 252, 0.2); */
        padding: 20px;
        padding-bottom: 0px;
    }

    
    
</style>