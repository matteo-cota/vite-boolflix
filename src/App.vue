<template>
  <div class="container">
    <h1 class="text-center my-4">Boolflix</h1>
    <!-- Search bar per cercare film -->
    <SearchBar @search="handleSearch" />
    <!-- Lista dei film trovati -->
    <MovieList :movies="movies" />
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
      movies: [], // Array di film che verranno visualizzati
    };
  },
  methods: {
    // Metodo per gestire la ricerca e fare la chiamata API
    handleSearch(query) {
      const apiKey = '3e71fbf202442d5cfffde584ebf5b815'; // 
      const apiUrl = `https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&query=${query}&language=it_IT`;

      // Chiamata all'API TheMovieDb
      fetch(apiUrl)
        .then(response => response.json())
        .then(data => {
          this.movies = data.results; // Aggiorna l'array dei film
        })
        .catch(error => console.error('Errore API:', error));
    },
  },
};
</script>