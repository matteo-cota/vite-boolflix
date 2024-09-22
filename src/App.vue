<template>
  <div class="container">
    <h1 class="text-center my-4">Boolflix</h1>
    
    <!-- Barra di ricerca per inserire il nome del film o della serie -->
    <SearchBar @search="handleSearch" />
    
    <!-- Mostra i risultati solo se ci sono film o serie TV -->
    <div v-if="movies.length > 0">
      <h2 class="my-4">Risultati:</h2>
      <MovieList :movies="movies" />
    </div>

    <!-- Messaggio di nessun risultato -->
    <div v-else class="text-center">
      <p>Nessun film o serie TV trovata. Prova con un'altra ricerca!</p>
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
      movies: [], // Lista di film e serie TV trovati
    };
  },
  methods: {
    // Gestisce la ricerca di film e serie TV tramite l'API
    handleSearch(query) {
      const apiKey = '3e71fbf202442d5cfffde584ebf5b815';
      const movieApiUrl = `https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&query=${query}&language=it_IT`;
      const tvApiUrl = `https://api.themoviedb.org/3/search/tv?api_key=${apiKey}&query=${query}&language=it_IT`;

      // Effettua le chiamate API per film e serie TV
      Promise.all([fetch(movieApiUrl), fetch(tvApiUrl)])
        .then(async ([movieResponse, tvResponse]) => {
          const moviesData = await movieResponse.json();
          const tvData = await tvResponse.json();
          
          // Combina i risultati dei film e delle serie TV
          this.movies = [...moviesData.results, ...tvData.results];
        })
        .catch(error => console.error('Errore API:', error));
    },
  },
};
</script>

<style>

</style>
