<script>
import axios from 'axios';

export default {
    name: 'Pokemon',
    emits: ['nombreIncorrecto', 'conteoActivos'],
    data() {
        return {
            pokemon: [],
            nombrePoke: '',
            //totalActivos: 0,
            // activo: false

        }
    },
    async mounted() {
        await this.listarPokemon()


    },
    computed: {
        totalActivos() {
            return this.pokemon.filter(poke => poke.activo === true).length;
        }
    },
    watch: {
        totalActivos(conteo) {
            this.$emit('conteoActivos', conteo);
        }
    },
    methods: {
        async getPokemon() {
            try{
                const url = 'https://pokeapi.co/api/v2/pokemon';
                const { data } = await axios.get(url);
                return data.results
            } catch (error){
                console.log("No se pudo obtener los pokemones",error);
            }

        },
        async getPicPokemon(pokeUrl) {
            const { data } = await axios.get(pokeUrl);
            return {
                name: data.name,
                pic: data.sprites.other.dream_world.front_default
            }
        },

        async listarPokemon() {
            const pokemonList = await this.getPokemon();
            const pokemonPic = await Promise.all(pokemonList.map(async (poke) => {
                const pokemonData = await this.getPicPokemon(poke.url);
                return {
                    name: pokemonData.name,
                    pic: pokemonData.pic,
                    nombrePoke: '',
                    activo: false
                };
            }));
            this.pokemon = pokemonPic;

            //console.log(this.pokemon);

        },
        verificarNombre(poke) {
            if (poke.nombrePoke.trim().toLowerCase() === poke.name) {
                poke.activo = true;
            } else {
                poke.activo = false;
                this.$emit('nombreIncorrecto');

            }
            poke.nombrePoke = '';

        }
    },
}
</script>

<template>
    <div class="container">
        <div class="pokemon" v-for="(pokemon, index) in pokemon" :key="index">
            <img :src="pokemon.pic" alt="foto de `{{pokemon.name}}`"
                :style="{ filter: pokemon.activo ? 'none' : 'blur(5px) grayscale(100%)' }">
            <div class="nombrePokemon" v-if="pokemon.activo">{{ pokemon.name }}</div>
            <input id="myInput" v-show="!pokemon.activo" type="text" v-model="pokemon.nombrePoke"
                @keyup.enter.prevent="verificarNombre(pokemon)" ref="myInput">
            <button v-show="!pokemon.activo" @click="verificarNombre(pokemon)">Descubrir</button>
        </div>
    </div>
    <div class="modal" tabindex="-1" id="myModal" ref="myModal">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Nombre incorrecto</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <p>El nombre ingresado no coincide con el del Pokémon.</p>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.container {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-template-rows: repeat(5, 1fr);
}

.pokemon {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin: 20px 0px 20px 0px;
}

img {
    width: 100px;
    height: 120px;

}

input {
    margin: 10px 0px 10px 0px;
}

.nombrePokemon {
    margin: 10px 0px 10px 0px;
}
</style>
