<template>
    <div id="modalDeath">
        <h3>
            Solo la muerte puede eliminar un turno.
        </h3>
        <h3>
            Demuestre que es usted.
        </h3>
        <form action="submit">
            <label>Que es lo que dice siempre cuando termina su danza?</label><br>
            <input name='text' type="text" v-model="text">
            <button @click="handleCheck">Enviar</button>
            <button @click="closeModal">Cancelar</button>
        </form>
    </div>
</template>

<script>
export default {
    name: 'ModalDeath',
    data(){
        return{
            text: '',
            isDeath: false    
        }
    },
    methods: {
        closeModal: function(e){
            e.preventDefault()
            this.$emit('closeModal')
        },
        handleCheck: function (e){
            e.preventDefault()
            if(this.text === 'Espero que la haya disfrutado'){
                this.isDeath = true
                this.$emit('isDeath')
                alert('Bienvenida, elimine el turno que usted desee')
                this.$emit('closeModal')
            }
            this.text = ''
        }
    },
    watch: {

    }
}
</script>

<style scoped>
    #modalDeath {
        background-color: rgba(3, 5, 7, 0.8);
        font-size: 20px;
        color: white;
        padding: 20px;
        border-radius: 10px;
    }
    h3{
        font-size: 30px;
        color: rgb(250, 200, 200);
        text-shadow: brown 1px 1px;
    }
    input {
        background-color: rgb(65, 21, 21, 0.8);
        border-radius: 4px;
        border: none;
        color: white;
        font-family: fantasy;
        margin: 10px;
        width: 300px;
        height: 30px;
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