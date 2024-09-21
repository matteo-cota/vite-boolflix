<template>
    <div class="row">
      <!-- Loop attraverso i film e visualizza ogni film con MovieItem -->
      <MovieItem v-for="movie in movies" :key="movie.id" :movie="movie" />
    </div>
  </template>
  
  <script>
  import MovieItem from './MovieItem.vue';
  
  export default {
    props: {
      movies: Array, // Array di film passato dal componente genitore
    },
    components: {
      MovieItem, // Importa il componente per visualizzare i dettagli del film
    },
  };
  </script>