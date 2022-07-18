<template>
  <div id="app">
    <h1>Rick And Morty</h1>
    <div class="container-input">
      <input
        type="text"
        @keyup="searchCharacter()"
        v-model="textSearch"
        class="input"
        placeholder="Search Character"
      />
    </div>
    <div class="container-characters">
      <div v-for="character in filteredCharacters" :key="character.id" class="card">
        <div><img :src="character.image" width="200" /></div>
        <div class="info">
          <h2>{{ character.name }}</h2>
          <p>{{ character.gender }}</p>
          <p :class="[character.status == 'Alive' ? 'alive' : 'dead']">
            {{ character.status }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  components: {},
  data() {
    return {
      characters: [],
      textSearch: "",
      filteredCharacters: [],
    };
  },
  mounted() {
    this.getData();
  },
  methods: {
    getData() {
      axios
        .get("https://rickandmortyapi.com/api/character")
        .then((response) => {
          this.characters = response.data.results;
          this.filteredCharacters = this.characters;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    searchCharacter() {
      this.filteredCharacters = this.characters.filter((character) => {
        return character.name
          .toLowerCase()
          .includes(this.textSearch.toLowerCase());
      });
    },
  },
};
</script>

<style>
#app {
  font-family: "Roboto", sans-serif;
  width: 100%;
  height: auto;
  background-color: #06262d;
}
h1 {
  color: #fff;
  text-align: center;
  font-size: 50px;
  text-shadow: 0 0 5px #36ff00, 0 0 10px #36ff00, 0 0 15px #36ff00;
}
.container-input {
  width: 100%;
  height: auto;
  background-color: unset;
  display: grid;
  place-items: center;
  margin-bottom: 20px;
}
.input {
  width: 80%;
  height: 50px;
  border: none;
  border-bottom: 2px solid #fff;
  background-color: #06262d;
  outline: none;
  color: #fff;
  font-size: 20px;
  text-shadow: 0 0 5px #36ff00;
}
.container-characters {
  width: 100%;
  height: auto;
  background-color: unset;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  place-items: center;
  grid-gap: 20px;
  grid-template-rows: 1fr;
  overflow: hidden;
  padding: 5%;
}
.card {
  display: flex;
  padding: 10px;
  background-color: #3c3d6e;
  border-radius: 10px;
  box-shadow: 0 0 5px #36ff00, 0 0 10px #36ff00, 0 0 15px #36ff00;
}
.info {
  text-align: center;
  display: grid;
  place-items: center;
  color: white;
  font-size: 20px;
}
.alive {
  color: #00ff00;
}
.dead {
  color: #ff0000;
}
</style>
