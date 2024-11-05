<script>
export default {
    props: {
        activos: {
            type: Number,
            default: 0
        }
    }
}
</script>

<template>
    <div class="header">
        <img class="logo" src="/src/assets/img/pokemon.png" alt="Logo Pokemon">
        <h2>¿Quién es ese Pokémon?</h2>
        <h4>Pokemones descubiertos: <span>{{ activos }}</span></h4>
    </div>
</template>

<style scoped>

.header {
    display: flex;
    align-items: center;
    flex-direction: column;
    margin-top: 50px;
    font-family: Arial, Helvetica, sans-serif;
}
.logo {
    width: 200px;
    height: auto;
}

span {
    font-weight: bold;
    color: rgb(180, 166, 37);
    font-size: large;
}
</style>