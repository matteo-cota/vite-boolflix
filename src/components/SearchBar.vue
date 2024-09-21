<template>
    <div class="input-group mb-3">
      <input 
        v-model="searchQuery" 
        type="text" 
        class="form-control" 
        placeholder="Cerca un film..."
      />
      <button class="btn btn-primary" @click="searchMovie">Cerca</button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        searchQuery: '',
      };
    },
    methods: {
      searchMovie() {
        // Emette l'evento 'search' con la query di ricerca
        this.$emit('search', this.searchQuery);
      },
    },
  };
  </script>