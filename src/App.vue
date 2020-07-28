<template>
  <div id="app">
    <template v-if="!loading">
      <ul>
        <li v-for="({ name, url, sprite }, index) in pokemons" :key="index">
          <h4>{{ name }}</h4>
          <img v-bind:src="sprite" alt="Imagem pokemon" />
          <a v-bind:href="url">{{ url }}</a>
        </li>
      </ul>
    </template>
    <h1 v-if="loading">Carregando....</h1>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return { pokemons: [], loading: true };
  },
  components: {},
  created: function() {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then(res => {
        res.data.results.forEach(pokemon => {
          //Requisitando url para puxar o sprite
          axios.get(pokemon.url).then(pokeInfo => {
            //Dando push para dentro do filted
            this.pokemons.push({
              ...pokemon,
              sprite: pokeInfo.data.sprites.front_default
            });
          });
        });
      })
      .finally(() => {
        this.loading = false;
      });
  }
};
</script>

<style scoped>
ul {
  display: flex;
  flex-flow: row wrap;
  justify-content: center;
  margin: 0;
  padding: 0;
}
li {
  flex-basis: 33%;
  text-transform: capitalize;
  display: flex;
  flex-flow: column;
  align-items: center;
}
</style>
