<template>
  <div :class="(view==='list')?'list':'grid'">
    <article v-for="(pokemon, index) in pokemons"
    :key="'pokemon'+index"
    @click="setPokemonUrl(pokemon.url)">
      <img :src="imageUrl + pokemon.id + '.png'" width="96" height="96" alt="">
      <h3>{{ pokemon.name }}</h3>
    </article>
  </div>
</template>

<script>
export default {
  props: [
    'imageUrl',
    'apiUrl',
    'view',
  ],
  data: () => ({
    pokemons: [],
  }),
  methods: {
    fetchData() {
      const req = new Request(this.apiUrl);
      fetch(req)
        .then((resp) => {
          if (resp.status === 200) {
            return resp.json();
          }
          return null;
        })
        .then((data) => {
          data.results.forEach((pokemon) => {
          // console.log('fetch, pokemon:', pokemon);
            const pokemonToReturn = pokemon;
            pokemonToReturn.id = pokemon.url.split('/')
              .filter((part) => !!part).pop();
            this.pokemons.push(pokemonToReturn);
          });
        })
        .catch((error) => {
          console.log(error);
        });
    },
    setPokemonUrl(url) {
      this.$emit('setPokemonUrl', url);
    },
  },
  created() {
    this.fetchData();
  },
};
</script>

<style lang="scss" scoped>
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  grid-gap: 10px;
  width: 80%;
  margin: auto;
}
.grid > article {
  height: 150px;
  background-color: #efefef;
  text-align: center;
  text-transform: capitalize;
  cursor: pointer;
}
.list {
  display: list;
  width: 80%;
  margin: auto;
}
.list > article {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #efefef;
  text-align: center;
  text-transform: capitalize;
  cursor: pointer;
  margin-bottom: 10px;
}
h3 {
  margin: 0;
}
</style>
