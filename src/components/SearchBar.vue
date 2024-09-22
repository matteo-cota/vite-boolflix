<template>
    <div class="input-group mb-3">
      <input
        type="text"
        class="form-control"
        placeholder="Cerca un film o una serie TV"
        v-model="searchQuery"
        @keydown.enter="emitSearch" 
      />
      <button class="btn btn-primary" @click="emitSearch">Cerca</button>
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
      emitSearch() {
        if (this.searchQuery.trim()) {
          this.$emit('search', this.searchQuery); 
        }
      },
    },
  };
  </script>
  
  <style scoped>
  
  </style>
  