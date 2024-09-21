<template>
  <div class="container">
    <h1 class="text-center my-4">Boolflix</h1>
    
    <!-- Barra di ricerca per inserire il nome del film -->
    <SearchBar @search="handleSearch" />
    
    <!-- Mostra i risultati solo se ci sono film -->
    <div v-if="movies.length > 0">
      <h2 class="my-4">Risultati:</h2>
      <MovieList :movies="movies" />
    </div>

    <!-- Messaggio di nessun risultato -->
    <div v-else class="text-center">
      <p>Nessun film trovato. Prova con un'altra ricerca!</p>
    </div>
  </div>
</template>

<script>
import SearchBar from './components/SearchBar.vue';
import MovieList from './components/MovieList.vue';

export default {
  components: {
    SearchBar,
    MovieList, 
  },
  data() {
    return {
      movies: [], // Lista dei film trovati
    };
  },
  methods: {
    // Gestisce la ricerca e fa una chiamata API
    handleSearch(query) {
      const apiKey = '3e71fbf202442d5cfffde584ebf5b815';
      const apiUrl = `https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&query=${query}&language=it_IT`;

      // Chiamata all'API
      fetch(apiUrl)
        .then(response => response.json())
        .then(data => {
          if (data.results) {
            this.movies = data.results; // Aggiorna i film trovati
          } else {
            this.movies = [];
          }
        })
        .catch(error => console.error('Errore API:', error));
    },
  },
};
</script>

<style>

</style>
