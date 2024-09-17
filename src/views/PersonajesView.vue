<template>
  <h1 class="text-center">Personajes</h1>

  <div class="d-flex justify-content-center mb-1">
    <div class="col-md-6 text-center m-2">
      <button v-if="prev != null" class="btn btn-warning" @click="anterior()">
        Anterior
      </button>
    </div>
    <div class="col-md-6 text-center m-2">
      <button v-if="next != null" class="btn btn-warning" @click="siguiente()">
        Siguiente
      </button>
    </div>
  </div>

  <div class="row justify-content-center">
    <div
      v-for="personaje in personajes"
      :key="personaje"
      class="col-md-3 mb-2 col-lg-2 col-sm-4"
    >
      <div class="card">
        <img
          class="card-img-top"
          :src="
            'https://starwars-visualguide.com/assets/img/characters/' +
            personaje.url.split('/')[5] +
            '.jpg'
          "
          alt="img"
        />
        <div class="card-body">
          <h5 class="card-title">
            {{ personaje.name }}
          </h5>
          <p class="card-text">
            <strong>Altura:</strong> {{ personaje.height }}
          </p>
          <p class="card-text"><strong>Peso:</strong> {{ personaje.mass }}</p>
          <p class="card-text">
            <strong>Color de ojos:</strong> {{ personaje.eye_color }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { ref, onMounted } from "vue";
const personajes = ref([]);

const prev = ref([]);
const next = ref([]);
onMounted(() => {
  cargarPersonajes();
});
const cargarPersonajes = async () => {
  try {
    const { data, count } = await axios.get("https://swapi.dev/api/people");
    personajes.value = data.results;
    console.log(personajes.value);

    prev.value = data.previous;
    next.value = data.next;
  } catch (error) {
    console.log(error);
  }
};

const siguiente = async () => {
  try {
    const { data } = await axios.get(next.value);
    personajes.value = data.results;
    prev.value = data.previous;
    next.value = data.next;
  } catch (error) {
    console.log(error);
  }
};

const anterior = async () => {
  try {
    const { data } = await axios.get(prev.value);
    personajes.value = data.results;
    prev.value = data.previous;
    next.value = data.next;
  } catch (error) {
    console.log(error);
  }
};
</script>

<style  scoped>
</style>