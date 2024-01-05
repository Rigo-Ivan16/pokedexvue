<template>
  <div>
    <div>
      <nav>
        <div class="container2">
          <div class="container-logo">
            <h2><span>Kantō</span>カントー地方<span>.</span></h2>
          </div>

          <ul class="links ">
            <router-link :to="{ name: 'UserProfile' }">
              <li class="link"><a href="#">Perfil</a></li>
            </router-link>
            <li class="link"><a href="#">Pokedex</a></li>
          </ul>
        </div>
      </nav>
    </div>

    <div class="center">
      <img :src="require('@/assets/pokemon3.png')" alt="NoFound" class="small-image">
    </div>

    <div class="content">
      <div class="search">
        <v-form @submit.prevent="searchPokemon">
          <v-text-field v-model="searchQuery.id" label="ID o Nombre"></v-text-field>
          <v-btn type="submit" color="primary">Buscar</v-btn>
          <v-btn v-if="show" @click="OriginalPokemonList" color="primary" class="ml-2">Regresar</v-btn>
        </v-form>
      </div>

      <v-app id="inspire" class="size">
        <v-card-group class="cards-container">
          <v-card v-for="(data, index) in paginatedPokemons" :key="index" class="mx-auto custom-card" max-width="200" height="300">
            <router-link :to="{ name: 'PokemonDetails', params: { id: data.id } }">
              <v-img class="pointer" :src="data.url" height="60%" cover></v-img>
              <v-card-title class="custom-title">{{ data.name }}</v-card-title>
              <v-card-subtitle class="custom-subtitle">{{ `Núm. de pokemón ${data.id}`}}</v-card-subtitle>
            </router-link>
          </v-card>
        </v-card-group>

        <v-pagination
          v-if="isActive && totalPages > 1 && !searching"
          v-model="page"
          :length="totalPages"
          @input="changePage"
          prev-icon="mdi-menu-left"
          next-icon="mdi-menu-right"
        ></v-pagination>
      </v-app>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
const API_URL = 'https://pokeapi.co/api/v2/pokemon/';

export default {
  data() {
    return {
      show: false,
      isActive: true,
      searching: false,
      pokemons: [],
      originalPokemons: [],
      itemsPerPage: 20,
      page: 1,
      searchQuery: {
        id: null,
        name: '',
      },
    };
  },
  computed: {
    totalPages() {
      return Math.ceil(this.pokemons.length / this.itemsPerPage);
    },
    paginatedPokemons() {
      const startIndex = (this.page - 1) * this.itemsPerPage;
      const endIndex = startIndex + this.itemsPerPage;
      return this.pokemons.slice(startIndex, endIndex);
    },
  },
  created() {
    this.InitialPokemons();
  },
  methods: {
    async InitialPokemons() {
      for (let i = 0; i < 151; i++) {
        await this.fetchPokemon(i + 1);
      }
      this.originalPokemons = [...this.pokemons];
    },
    async fetchPokemon(pokemonId) {
      try {
        const response = await axios.get(`${API_URL}${pokemonId}`);
        const pokemon = {
          id: response.data.id,
          name: response.data.name,
          url: response.data.sprites.front_default,
        };
        this.pokemons.push(pokemon);
      } catch (error) {
        console.log(error);
      }
    },
    async searchPokemon() {
      const searchTerm = this.searchQuery.id || this.searchQuery.name.trim().toLowerCase();
      try {
        this.pokemons = [];
        this.searching = true;
        if (searchTerm !== '') {
          const response = await axios.get(`${API_URL}${searchTerm}`);
          this.handleSearchResponse(response);
        } else {
          this.OriginalPokemonList();
        }
      } catch (error) {
        console.error(error);
        this.handleSearchError(error);
      } finally {
        this.searching = false;
      }
    },
    showPagination(isActive) {
      this.isActive = isActive;
    },
    handleSearchResponse(response) {
      const pokemon = {
        id: response.data.id,
        name: response.data.name,
        url: response.data.sprites.front_default,
      };
      if (pokemon.id <= 151) {
        this.pokemons.push(pokemon);
        this.show = true;
      } else {
        alert('¡Error! La búsqueda está limitada hasta el Pokémon con ID 151.');
        this.OriginalPokemonList();
      }
    },
    handleSearchError(error) {
      this.show = false;
      if (error.response && error.response.status === 404) {
        alert('¡Error! Pokémon no encontrado. Por favor, verifica el nombre o ID ingresado.');
      } else {
        alert('¡Error! Ocurrió un problema al buscar el Pokémon.');
      }
      this.OriginalPokemonList();
    },
    OriginalPokemonList() {
      this.pokemons = [];
      this.InitialPokemons().then(() => {
        this.show = false;
      });
    },
    changePage(page) {
      this.page = page;
    },
  },
};
</script>

<style scoped>
.center {
  text-align: center;
  margin-top: 2%;
}

.small-image {
  width: 30%;
  max-width: 100%;
}

.size {
  height: auto;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.custom-card {
  border-radius: 12px;
  margin-bottom: 20px;
  width: 100%;
  box-sizing: border-box; 
  transition: box-shadow 0.3s ease, transform 0.3s ease;
}
.custom-card:hover {
  box-shadow: 0 5px 15px rgba(3, 89, 92, .5);
  transform: translateY(-15px);
}
.custom-title,
.custom-subtitle {
  text-decoration: none;
  color: black;
}

.search {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 30px;
}

nav {
  background: #021222;
  height: 80px;
  position: relative;
}

.container2 {
  margin: auto; 
  display: flex;
  justify-content: space-between;
  max-width: 1000px;
  line-height: 80px;
  z-index: 100;
}

.container-logo {
  margin-left: 20px;
  z-index: 100;
  cursor: pointer;
}

.container-logo h2 {
  color: #95bdd8;
  font-weight: 300;
  font-size: 35px;
  letter-spacing: 2px;
}

.container-logo h2 span {
  color: #0590ec;
  font-weight: 700;
}

.links .link {
  display: inline-block;
}

.links .link a {
  text-decoration: none;
  color: #d1faf4;
  font-size: 20px;
  margin: 0 25px;
  letter-spacing: 3px;
  transition: .3s;
}

.links .link a:hover {
  color: #487abb;
}

@media screen and (max-width: 800px){
  .links {
      position: absolute;
      display: flex;
      flex-direction: column;
      background: #021222;
      text-align: center;
      overflow: hidden;
      height: calc(100vh - 80px);
      width: 100%;
      z-index: -1;
      top: -100vh;
      transition: .5s;
  }
  .active {
      top: 80px;
  }
  .links .link:hover {
      background: #051c38;
  }
}
</style>
