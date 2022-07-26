<template>
  <div>
    <div id="pokemonModal" class="modal" @click="closeDetail">
      <!-- Modal content -->
      <div class="modal-content" v-if="show" @click.stop="">
        <span class="close" @click="closeDetail">&times;</span>
        <div class="pokemon">
          <div class="pokemon-left">
            <div>
              <img :src="imageUrl + pokemon.id + '.png'" width="96" height="96" alt="">
            </div>
            <div> #{{ pokemon.id }} </div>
          </div>
          <div class="pokemon-right">
            <h3> {{ pokemon.name }}</h3>
            <div> Type:
              <span class="capital" v-for="(value, index) in pokemon.types" :key="'value'+index">
                <span v-if="index > 0"> / </span>
                {{ value.type.name }}
              </span>
            </div>
            <div> Height: {{pokemon.height}}0 cm</div>
            <div> Weight: {{pokemon.weight/100}} kg</div>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
export default {
  props: [
    'pokemonUrl',
    'imageUrl',
  ],
  data: () => (
    {
      show: false,
      pokemon: {},
    }
  ),
  methods: {
    fetchData() {
      const req = new Request(this.pokemonUrl);
      fetch(req)
        .then((resp) => {
          if (resp.status === 200) {
            return resp.json();
          }
          return null;
        })
        .then((data) => {
          // console.log('fetch data: ', data);
          this.pokemon = data;
          this.show = true;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    closeDetail() {
      this.$emit('closeDetail');
    },
  },
  created() {
    this.fetchData();
  },
};
</script>

<style lang="scss" scoped>
.pokemon {
  display:flex;
  width: 100%;
  justify-content: center;
  padding-bottom: 10%;
}

.capital, h3 {
  text-transform: capitalize;
}

.pokemon-right {
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
}

.pokemon-left {
  margin-left: -7%;
}

.modal {
  display: block; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0,0,0); /* Fallback color */
  background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
}

/* Modal Content/Box */
.modal-content {
  background-color: #fefefe;
  margin: 15% auto; /* 15% from the top and centered */
  padding: 20px;
  border: 1px solid #888;
  width: 50%; /* Could be more or less, depending on screen size */
}

/* The Close Button */
.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}
</style>
