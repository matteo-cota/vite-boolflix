<template>
    <div class="col-md-4 mb-4">
      <div class="card">
        <div class="card-body">
          <h5 class="card-title">{{ movie.title }}</h5>
          <h6 class="card-subtitle mb-2 text-muted">Titolo Originale: {{ movie.original_title }}</h6>
          <p class="card-text">Lingua: {{ movie.original_language }}</p>
          <p class="card-text">Voto: {{ movie.vote_average }}</p>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      movie: Object, // Oggetto singolo film passato come props
    },
  };
  </script>