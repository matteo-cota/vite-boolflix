<template>
  <div id="app">
    <!-- Header con logo e nav bar -->
    <header class="bg-dark text-light py-3 mb-4">
      <div class="container d-flex justify-content-between align-items-center">
        <div class="d-flex align-items-center">
          <img src="/src/img/boolflix.png" alt="Logo Boolflix">
          <nav>
            <ul class="nav">
              <li class="nav-item">
                <a class="nav-link text-light" href="#" @click="loadHome">Home</a>
              </li>
              <li class="nav-item">
                <a class="nav-link text-light" href="#" @click="loadMovies">Film</a>
              </li>
              <li class="nav-item">
                <a class="nav-link text-light" href="#" @click="loadTvShows">Serie TV</a>
              </li>
            </ul>
          </nav>
        </div>
        <div class="d-flex align-items-center">
          <SearchBar @search="handleSearch" />
          <i class="fa-solid fa-bell"></i>
          <button class="btn btn-light ms-2">Account</button>
        </div>
      </div>
    </header>

    <main class="container mt-4">
      <!-- Sezione Home: mostra sia film che serie TV -->
      <div v-if="currentSection === 'home'" class="row g-4">
        <h3 class="text-light mb-4">Film</h3>
        <MovieList :movies="movies" />

        <h3 class="text-light mb-4">Serie TV</h3>
        <MovieList :movies="tvShows" />
      </div>

      <!-- Sezione Film -->
      <div v-if="currentSection === 'movies' && movies.length > 0" class="row g-4">
        <h3 class="text-light mb-4">Film</h3>
        <MovieList :movies="movies" />
      </div>

      <!-- Sezione Serie TV -->
      <div v-if="currentSection === 'tvShows' && tvShows.length > 0" class="row g-4">
        <h3 class="text-light mb-4">Serie TV</h3>
        <MovieList :movies="tvShows" />
      </div>

      <!-- Messaggio se non ci sono risultati -->
      <div v-if="(currentSection === 'home' && movies.length === 0 && tvShows.length === 0) || (currentSection === 'movies' && movies.length === 0) || (currentSection === 'tvShows' && tvShows.length === 0)" class="text-center text-light mt-4">
        <p>Nessun film o serie TV trovata. Prova con un'altra ricerca!</p>
      </div>
    </main>

    <!-- Footer con icone social -->
    <footer class="bg-dark text-light py-3 mt-4">
      <div class="container text-center">
        <p>&copy; 2024 Boolflix. Tutti i diritti riservati.</p>
        <div class="social-icons">
          <a href="#" class="text-light me-3"><i class="fab fa-facebook"></i></a>
          <a href="#" class="text-light me-3"><i class="fab fa-twitter"></i></a>
          <a href="#" class="text-light me-3"><i class="fab fa-instagram"></i></a>
          <a href="#" class="text-light"><i class="fab fa-linkedin"></i></a>
        </div>
      </div>
    </footer>
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
      movies: [],
      tvShows: [],
      currentSection: 'home',
      apiKey: '3e71fbf202442d5cfffde584ebf5b815',
    };
  },
  methods: {
    fetchMovies(query) {
      const movieApiUrl = `https://api.themoviedb.org/3/search/movie?api_key=${this.apiKey}&query=${query}&language=it_IT`;

      fetch(movieApiUrl)
        .then(response => response.json())
        .then(data => {
          this.movies = data.results || [];
          this.currentSection = 'home'; 
        })
        .catch(error => console.error('Errore API film:', error));
    },

    fetchTvShows(query) {
      const tvApiUrl = `https://api.themoviedb.org/3/search/tv?api_key=${this.apiKey}&query=${query}&language=it_IT`;

      fetch(tvApiUrl)
        .then(response => response.json())
        .then(data => {
          this.tvShows = data.results || [];
          this.currentSection = 'home'; 
        })
        .catch(error => console.error('Errore API serie TV:', error));
    },

    handleSearch(query) {
      this.fetchMovies(query);
      this.fetchTvShows(query);
    },

    loadMovies() {
      this.currentSection = 'movies';
      const popularMoviesApiUrl = `https://api.themoviedb.org/3/movie/popular?api_key=${this.apiKey}&language=it_IT`;

      fetch(popularMoviesApiUrl)
        .then(response => response.json())
        .then(data => {
          this.movies = data.results || [];
          this.tvShows = []; // Reset serie TV
        })
        .catch(error => console.error('Errore API film popolari:', error));
    },

    loadTvShows() {
      this.currentSection = 'tvShows';
      const popularTvShowsApiUrl = `https://api.themoviedb.org/3/tv/popular?api_key=${this.apiKey}&language=it_IT`;

      fetch(popularTvShowsApiUrl)
        .then(response => response.json())
        .then(data => {
          this.tvShows = data.results || [];
          this.movies = []; // Reset film
        })
        .catch(error => console.error('Errore API serie TV popolari:', error));
    },

    loadHome() {
      this.currentSection = 'home';
      this.loadMovies(); // Carica i film popolari
      this.loadTvShows(); // Carica le serie TV popolari
    },
  },
  mounted() {
    this.loadHome(); // Carica home al montaggio del componente
  },
};
</script>



<style scoped>
/* Stili per il layout */
header {
  background-color: #141414;
}

.nav {
  list-style: none;
  padding: 0;
  margin: 0;
}

.fa-bell {
  margin: 10px;
}

.nav-item {
  margin-right: 15px;
}

.btn-light {
  background-color: #333;
  color: white;
  margin: 10px;
}

.btn-light:hover {
  background-color: #444;
}

.container {
  max-width: 1400px;
}
</style>
