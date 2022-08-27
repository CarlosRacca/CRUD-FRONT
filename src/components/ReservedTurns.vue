<template>
    <div id="availableTurns">
        <br>
        
        Los turnos ya reservados para bailar con la muerte son:
        <table class="table">
            <thead class="tableHead">
                <th v-for="(element) in this.turnsGrouped"
                    :key="element.id">
                        <tr>
                            {{element.date}}
                        </tr>    
                    </th>
            </thead>
            <tbody class="tableBody">
                <td v-for="(element) in this.turnsGrouped"
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
            console.log(this.api.data)
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
                console.log('Se eleminof')
                this.turns = []
                this.api = await axios.get('https://crud-dance-api.herokuapp.com/api/turnos')
            }
            this.deletedTurn = ''
        },

        newTurn: async function(){
            console.log(this.newTurn)
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
            axios.delete('https://crud-dance-api.herokuapp.com/api/turnos/' + e.target.id)
            alert('Has eliminado el turno de ' + e.target.name)
            this.deletedTurn = 'Turno eliminado'
        },

        sortArrayTimes: function(x, y){
            if (x.time < y.time) {return -1;}
            if (x.time > y.time) {return 1;}

            return 0;
        }

    }   
}
</script>

<style>
    #availableTurns {
        color: white;
        background-color: rgb(122, 153, 122, 0.4);
    }
</style>