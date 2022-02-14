<template>
  <div>
    <div class="hero is-white is-gradient is-bold" id="hero">
      <div class="hero-body">
        <h1 class="title">
          <span class="has-text-success">R&M</span>
          <span class="subtitle">Personajes</span>
        </h1>
        <div class="field has-addons is-pulled-right">
          <div class="control">
            <input
              v-model="search"
              type="text"
              class="input is-rounded"
              placeholder="Nombre del personaje"
              v-on:keyup.enter="searchData"
            />
          </div>

          <div class="control">
            <button
              class="button is-success is-rounded"
              v-on:click="searchData"
            >
              Buscar
            </button>
          </div>
        </div>
      </div>
    </div>

    <div class="container">
      <div
        class="columns is-desktop is-mobile is-tablet is-multiline is-centered"
      >
        <character
          @showModal="showModal"
          v-for="character of characters"
          v-bind:key="character.id"
          v-bind:character="character"
        />
      </div>

      <nav
        class="pagination is-rounded"
        role="navegation"
        aria-label="pagination"
      >
        <a class="pagination-previous" v-on:click="changePage(page - 1)"
          >Anterior</a
        >

        <ul class="pagination-list">
          <li>
            <a class="pagination-link is-current">{{ page }}</a>
          </li>
        </ul>

        <a class="pagination-next" v-on:click="changePage(page + 1)"
          >Siguiente</a
        >
      </nav>
    </div>

    <div class="modal" :class="{ 'is-active': modal }" v-if="modal">
      <div class="modal-background" @click="modal = false"></div>
      <div class="modal-card">
        <header class="modal-card-head">
          <p class="modal-card-title">Acerca de: {{ currentCharacter.name }}</p>
          <button
            class="delete"
            aria-label="close"
            @click="modal = false"
          ></button>
        </header>
        <section class="modal-card-body">
          <div class="field">
            <label class="label">Genero</label>
            <div class="control">
              <input class="input" type="text" :value=" currentCharacter.gender " readonly/>
            </div>
          </div>
          <div class="field">
            <label class="label">Estado</label>
            <div class="control">
              <input class="input" type="text"  :value="currentCharacter.status" readonly/>
            </div>
          </div>
          <div class="field">
            <label class="label">Especie</label>
            <div class="control">
              <input class="input" type="text" :value="currentCharacter.species " readonly/>
            </div>
          </div>
          <div class="field">
            <label class="label">Tipo</label>
            <div class="control">
              <input class="input" type="text" :value=" currentCharacter.type " readonly/>
            </div>
          </div>
        </section>
        <footer class="modal-card-foot"></footer>
      </div>
    </div>
  </div>
</template>

<style scoped>
#hero {
  margin-bottom: 10px;
}
.pagination {
  margin: 10px;
}
</style>

<script>
import axios from "axios";

import Character from "./components/Character";

export default {
  name: "App",
  components: {
    Character,
  },
  data: function () {
    return {
      characters: [],
      page: 1,
      pages: 1,
      search: "",
      modal: false,
      currentCharacter: {},
    };
  },
  created() {
    this.cargar();
  },
  methods: {
    async cargar() {
      const params = {
        page: this.page,
        name: this.search,
      };

      await axios
        .get("https://rickandmortyapi.com/api/character/", { params })
        .then((response) => {
          this.characters = response.data.results;
          this.pages = response.data.info.pages;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    changePage(page) {
      this.page = page <= 0 || page > this.pages ? this.page : page;
      this.cargar();
    },
    searchData() {
      this.page;
      this.cargar();
    },
    showModal(id) {
      // consultar datos del personaje elegido
      this.cargarUno(id);
    },
    async cargarUno(id) {
      // llamado HTTP
      let result = await axios.get(
        `https://rickandmortyapi.com/api/character/${id}`
      );
      // asignar datos al modal
      this.currentCharacter = result.data;
      this.modal = true;
    },
  },
};
</script>