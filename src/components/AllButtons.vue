<template>
    <div id="firstButtons">
        
        <button @click="showModalSelect">Seleccionar un turno</button>
        <h1 id="title">DANZA DE LA MUERTE</h1>
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

<style scoped>
    #firstButtons {
        color: white;
        display: flex;
        justify-content: space-between;
        align-items: center;
        /* margin-top: 0px; */
        margin-top: -50px;
    }

    button{
        color: white;
        background-color: rgb(113, 27, 27);
        border: none;
        border-radius: 5px;
        margin: 20px;
        box-shadow: black 3px 3px;
        height: 50px;
        width: 230px;
        padding-top: 10px;
        font-size: 20px;
        font-family: fantasy;
        cursor: pointer;

    }
    button:hover{
        background-color: rgb(26, 26, 26);
        box-shadow: black 0px 0px;
    }

    h1{
        margin-top: 60px;
        color: rgb(123, 34, 34);
        font-size: 60px;
        text-shadow: black 2px 2px;
    }
</style>