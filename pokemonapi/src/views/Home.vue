<template>
  <div class="home">
    <h1>PokemonAPI by Isaak </h1>
    <div class="viewToggle">
      <button @click="toggleView">
        <span v-if="view === 'grid'">List View</span>
        <span v-else>Grid View</span>
      </button>
    </div>
    <PokemonList :view="view" :imageUrl="imageUrl" :apiUrl="apiUrl" @setPokemonUrl="setPokemonUrl"/>
    <PokemonDetail
      v-if="showDetail"
      :pokemonUrl="pokemonUrl"
      :imageUrl="imageUrl"
      @closeDetail="closeDetail"
    />
  </div>
</template>

<script>
import PokemonList from '@/components/PokemonList.vue';
import PokemonDetail from '@/components/PokemonDetail.vue';

export default {
  data: () => ({
    imageUrl:
      'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/',
    apiUrl: 'https://pokeapi.co/api/v2/pokemon/?limit=151',
    pokemonUrl: '',
    showDetail: false,
    view: 'grid',
  }),
  components: {
    PokemonList,
    PokemonDetail,
  },
  methods: {
    setPokemonUrl(url) {
      this.pokemonUrl = url;
      this.showDetail = true;
    },
    closeDetail() {
      this.pokemonUrl = '';
      this.showDetail = false;
    },
    toggleView() {
      if (this.view === 'grid') {
        this.view = 'list';
      } else {
        this.view = 'grid';
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.viewToggle{
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
  margin-right: 10%;
  margin-bottom: 1%;
}
</style>
