<template>
    <div id="modalDeath">
        Solo la muerte puede eliminar un turno para la danza.

        Demuestre que usted es la muerte.

        <form action="submit">
            <label>Que dice la muerte cuando termina su danza?</label><br>
            <input name='text' type="text" v-model="text">
            <button @click="closeModal">Cancelar</button>
            <button @click="handleCheck">Enviar</button>
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

<style>
    #modalDeath {
        background-color: brown;
    }
</style>