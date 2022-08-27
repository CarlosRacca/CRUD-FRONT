<template>
    <div id="firstButtons">
        <br>
        <button @click="showModalSelect">Seleccionar un turno</button>
        <button @click="showModalDeath">Eliminar un turno</button>
    </div>
</template>

<script>
export default {
    name: 'AllButtons',
    methods: {
        showModalSelect: function(e){
            e.preventDefault()
            this.$emit('showModalSelect')

        },
        showModalDeath: function(e){
            e.preventDefault()
            this.$emit('showModalDeath')
        },
    } 
}
</script>

<style>
    #firstButtons {
        background-color: aqua;
    }
</style>