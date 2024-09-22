<template>
    <div class="col-md-4 mb-4">
      <div class="card h-100">
        <!-- Aggiungiamo l'immagine di copertina del film o serie TV -->
        <img 
          class="card-img-top" 
          :src="getImageUrl(movie.poster_path)" 
          alt="Poster" 
          v-if="movie.poster_path"
        />
        
        <div class="card-body">
          <h5 class="card-title">{{ movie.title || movie.name }}</h5>
          <h6 class="card-subtitle mb-2 text-muted">Titolo Originale: {{ movie.original_title || movie.original_name }}</h6>
  
          <!-- Mostra la bandiera della lingua -->
          <p class="card-text">
            Lingua: 
            <img :src="getFlagUrl(movie.original_language)" alt="Flag" width="30" class="me-2" />
            {{ movie.original_language.toUpperCase() }}
          </p>
  
          <!-- Mostra il voto come stelle -->
          <p class="card-text">
            Voto: 
            <span v-for="n in getStarCount(movie.vote_average)" :key="n" class="fa fa-star text-warning"></span>
            <span v-for="n in (5 - getStarCount(movie.vote_average))" :key="n" class="fa fa-star-o text-warning"></span>
          </p>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      movie: Object, // Riceve i dettagli del singolo film o serie
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
        // Restituisci l'URL dell'immagine della bandiera
        const flagCode = flagMap[language] || 'un'; // Se la lingua non esiste, mostra una bandiera "non definita"
        return `https://flagcdn.com/h40/${flagCode}.png`;
      },
  
      // Metodo per ottenere l'URL dell'immagine di copertina
      getImageUrl(posterPath) {
        const baseUrl = 'https://image.tmdb.org/t/p/w342';
        return posterPath ? `${baseUrl}${posterPath}` : '';
      },
  
      // Trasforma il voto da 1-10 in un numero di stelle da 1 a 5
      getStarCount(voteAverage) {
        return Math.ceil(voteAverage / 2); // Arrotonda per eccesso
      },
    },
  };
  </script>
  
  <style scoped>
  .card {
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }
  .flag-icon {
    font-size: 20px;
  }
  .fa-star,
  .fa-star-o {
    font-size: 18px;
  }
  </style>
  