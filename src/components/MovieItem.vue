<template>
  <div class="col-md-3">
    <div 
      class="movie-card position-relative" 
      @mouseover="showDetails = true" 
      @mouseleave="showDetails = false"
      :style="{ backgroundImage: `url(https://image.tmdb.org/t/p/w342/${movie.poster_path})` }"
    >
      <!-- Dettagli visibili solo al passaggio del mouse -->
      <div v-if="showDetails" class="movie-details p-3 text-light">
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
        
        <!-- Stelle in base al voto -->
        <div class="stars">
          <span v-for="n in 5" :key="n" class="me-1">
            <i :class="n <= filledStars ? 'fas fa-star' : 'far fa-star'"></i>
          </span>
        </div>

        <p>{{ movie.overview }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    movie: Object, // Riceve i dettagli del singolo film o serie
  },  
  
  data() {
    return {
      showDetails: false, // Stato per mostrare/nascondere i dettagli al passaggio del mouse
    };
  },

  computed: {
    // Calcola il numero di stelle piene in base al voto
    filledStars() {
      return Math.ceil(this.movie.vote_average / 2); // Arrotonda il voto da 1 a 5 stelle
    },
  },
  
  methods: {
    // Metodo per ottenere l'URL della bandiera in base al codice lingua
    getFlagUrl(language) {
      const flagMap = {
        us: 'us',  // Stati Uniti  
        en: 'gb',  // Inglese          
        it: 'it',  // Italiano
        fr: 'fr',  // Francese
        es: 'es',  // Spagnolo
        pt: 'pt',  // Portoghese
        de: 'de',  // Tedesco
        ja: 'jp',  // Giapponese
        ko: 'kr',  // Coreano
        zh: 'cn',  // Cinese
      };
      const flagCode = flagMap[language] || 'un'; // Se la lingua non esiste, mostra una bandiera "non definita"
      return `https://flagcdn.com/h40/${flagCode}.png`;
    },
  },
};
</script>

<style scoped>
/* Stili per la card del film */
.movie-card {
  background-size: cover;
  background-position: center;
  height: 400px;
  margin-bottom: 20px;
  border-radius: 10px;
  position: relative;
  overflow: hidden;
}

.movie-details {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  flex-direction: column;
  justify-content: center;
  opacity: 0;
  transition: opacity 0.3s;
  font-size: 11px;
}

.movie-card:hover .movie-details {
  opacity: 1;
}

/* Stili per le stelle */
.stars {
  display: flex;
}

.fas.fa-star {
  color: #ffc107; 
}

.far.fa-star {
  color: #ccc; /* Grigio per le stelle vuote */
}

.flag {
  width: 20px; /* Dimensione della bandiera */
  height: auto;
  margin-left: 5px; /* Spazio tra la lingua e la bandiera */
}
</style>
