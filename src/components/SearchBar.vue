<template>
  <div class="input-group d-flex justify-content-between align-items-center">
    <input
    type="text"
    class="form-control font search-input"
    placeholder="Cerca film o una serie TV"
    v-model="searchQuery"
    @keydown.enter="emitSearch"
    />
    <button class="btn btn-danger" @click="emitSearch">Cerca</button>
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

input.form-control {  
   border: none;     
}

input.form-control:focus {
  outline: none;
  box-shadow: none;  
}
</style>
