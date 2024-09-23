<template>
  <div 
    class="movie-card position-relative" 
    @mouseover="fetchCastAndGenres" 
    @mouseleave="showDetails = false"
    :style="{ backgroundImage: `url(https://image.tmdb.org/t/p/w342/${movie.poster_path})` }"
  >
    <div v-if="showDetails" class="movie-details p-3 text-light">
      <!-- Dettagli del film o serie -->
      <h5>{{ movie.title || movie.name }}</h5>
      <p>Titolo Originale: {{ movie.original_title || movie.original_name }}</p>
      <p>
        Lingua: 
        <img 
          :src="getFlagUrl(movie.original_language)" 
          alt="Bandiera" 
          class="flag" 
        />
      </p>
      <div class="stars">
        <span v-for="n in 5" :key="n" class="me-1">
          <i :class="n <= filledStars ? 'fas fa-star' : 'far fa-star'"></i>
        </span>
      </div>
      <p>{{ movie.overview }}</p>
      <p>Generi: {{ genres }}</p>
      <p>Cast:</p>
      <ul>
        <li v-for="actor in cast" :key="actor.id">{{ actor.name }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    movie: Object,
  },
  data() {
    return {
      showDetails: false,
      cast: [],
      genres: '',
    };
  },
  computed: {
    filledStars() {
      return Math.ceil(this.movie.vote_average / 2);
    },
  },
  methods: {
    getFlagUrl(language) {
      const flagMap = {
        us: 'us',
        en: 'gb',
        it: 'it',
        fr: 'fr',
        es: 'es',
        pt: 'pt',
        de: 'de',
        ja: 'jp',
        ko: 'kr',
        zh: 'cn',
      };
      const flagCode = flagMap[language] || 'un';
      return `https://flagcdn.com/h40/${flagCode}.png`;
    },

    fetchCastAndGenres() {
      this.showDetails = true; // Mostra i dettagli al passaggio del mouse
      const apiKey = '3e71fbf202442d5cfffde584ebf5b815';
      const movieId = this.movie.id;
      const mediaType = this.movie.media_type || (this.movie.title ? 'movie' : 'tv');

      const apiUrl = `https://api.themoviedb.org/3/${mediaType}/${movieId}/credits?api_key=${apiKey}&language=it_IT`;

      fetch(apiUrl)
        .then(response => response.json())
        .then(data => {
          this.cast = data.cast.slice(0, 5);
          this.genres = this.movie.genre_ids.map(id => {
            const genreMap = {
              28: 'Azione',
              12: 'Avventura',
              16: 'Animazione',
              35: 'Commedia',
              80: 'Crimine',
              99: 'Documentario',
              18: 'Drammatico',
              10751: 'Familiare',
              14: 'Fantasy',
              36: 'Storia',
              27: 'Horror',
              10402: 'Musica',
              9648: 'Mystery',
              10749: 'Romantico',
              878: 'Fantascienza',
              10770: 'Film TV',
              53: 'Thriller',
              10752: 'Guerra',
              37: 'Western',
            };
            return genreMap[id] || 'Sconosciuto';
          }).join(', ');
        })
        .catch(error => console.error('Errore API cast:', error));
    },
  },
};
</script>

<style scoped>
.movie-card {
  width: 300px; 
  height: 450px;
  padding: 10px;
  margin-bottom: 30px;
  margin-left: 40px;
  background-size: cover; 
  background-position: center; 
  border-radius: 10px; 
  overflow: hidden;
  transition: transform 0.3s ease-in-out;
}

.movie-card:hover {
  transform: scale(1.05); 
}

.movie-details {
  font-size: 0.6em;
  height: 100%;
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba(0, 0, 0, 0.7);
  display: none;
  padding: 10px;
}

.movie-card:hover .movie-details {
  display: block; 
}

.flag {
  width: 20px;
  height: auto;
}

.stars i {
  color: #FFD700;
}
</style>
