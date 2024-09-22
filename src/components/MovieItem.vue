<template>
    <div class="col-md-4 mb-4">
      <div class="card h-100">
        <div class="card-body">
          <h5 class="card-title">{{ movie.title || movie.name }}</h5>
          <h6 class="card-subtitle mb-2 text-muted">Titolo Originale: {{ movie.original_title || movie.original_name }}</h6>
          
          <!-- Mostra la bandiera della lingua -->
          <p class="card-text">
            Lingua: 
            <img :src="getFlagUrl(movie.original_language)" alt="Flag" width="30" class="me-2" />
            {{ movie.original_language.toUpperCase() }}
          </p>
          
          <p class="card-text">Voto: {{ movie.vote_average }}</p>
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
          en: 'us',  // Stati Uniti
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
    },
  };
  </script>
  