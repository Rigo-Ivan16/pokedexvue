<template>
  <div>
    <div class="card">
      <div class="head">
        <div class="circle"></div>
        <div class="img">
          <img :src="pokemonDetails.image" :alt="pokemonDetails.name">
        </div>
      </div>
      <br>
      <div class="description">
        <h3 class="pokemon-name">{{ pokemonDetails.name }}</h3>
        <h4>ID: {{ pokemonDetails.id }}</h4>
        <p>Tipo: {{ pokemonDetails.type }}</p>
        <p>Poder: {{ pokemonDetails.power }}</p>
        <p>Habilidades:</p>
        <ul>
          <li v-for="(ability, index) in pokemonDetails.abilities" :key="index">{{ ability }}</li>
        </ul>
      </div>
      <div class="contact">
        <router-link to="/" class="back-link">
          <a class="btn">Regresar</a>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      pokemonDetails: {},
      error: null,
    };
  },
  created() {
    const pokemonId = this.$route.params.id;
    this.fetchPokemonDetails(pokemonId);
  },
  methods: {
    async fetchPokemonDetails(id) {
      try {
        const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`);
        const data = response.data;
        this.pokemonDetails = {
          id: data.id,
          name: data.name,
          type: data.types[0].type.name,
          power: data.base_experience,
          abilities: data.abilities.slice(0, 4).map(ability => ability.ability.name),
          image: data.sprites.front_default,
        };
      } catch (error) {
        console.error(error);
        this.error = 'Error al obtener los detalles del Pokémon. Por favor, inténtelo de nuevo más tarde.';
      }
    },
  },
};
</script>


<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Baloo 2', cursive;
}

body {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  background: #efefef;
  flex-wrap: wrap;
}

.card {
  position: relative;
  width: 300px;
  height: 535px; /* Ajustado para mostrar toda la información */
  background: #fff;
  border-radius: 8px;
  overflow: hidden;
  transition: .5s;
  margin: 15px;
  margin-top: 53px;
}

.card:hover {
  box-shadow: 0 5px 15px rgba(3, 89, 92, .5);
  transform: translateY(-15px);
}

.card .head {
  height: 125px;
  width: 100%;
  position: relative;
}

.card .head .circle {
  position: absolute;
  width: 300px;
  height: 300px;
  border-radius: 50%;
  background: #021222;
  bottom: 0;
}

.card .head .img {
  width: 150px;
  height: 150px;
  position: absolute;
  background: #fff;
  padding: 5px;
  border-radius: 50%;
  bottom: -30%;
  left: 50%;
  transform: translate(-50%);
}

.card .head .img img {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  object-fit: cover;
}

.card .description {
  height: 300px; /* Ajustado para mostrar toda la información */
  padding: 20px;
  border-bottom: solid 1px rgba(6, 74, 76, .18);
  text-align: center;
}

.card .description h3 {
  color: #05383a;
}

.card .description h4 {
  color: #021222;
}

.card .description p {
  margin-top: 20px;
  font-size: 15px;
}

.card .contact {
  width: 100%;
  height: 75px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.card .contact a {
  text-decoration: none;
  color: #efefef;
  background: #021222;
  padding: 5px 20px;
  border-radius: 5px;
  transition: .3s;
}

.card .contact a:hover {
  background: #012627;
}
.v-application.theme--light {
  background: none;
  color: inherit;
}
</style>
