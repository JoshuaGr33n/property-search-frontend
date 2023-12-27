<template>
    <div>
      <div class="container mt-4">
        <h1 class="mb-4">{{ searchParams.pageTitle }}</h1>
        <form>
          <div class="form-group">
            <label for="selectParam">Select Parameter:</label>
            <select class="form-control" v-model="selectedParam" id="selectParam">
              <option value="name">Name</option>
              <option value="bedrooms">Bedrooms</option>
              <option value="bathrooms">Bathrooms</option>
              <option value="storeys">Storeys</option>
              <option value="garages">Garages</option>
              <option value="min_price">Min Price</option>
              <option value="max_price">Max Price</option>
            </select>
          </div>
          <div class="form-group">
            <input class="form-control" v-model="searchParams[selectedParam]" placeholder="Enter search term" />
          </div>
          <button type="button" class="btn btn-primary" @click="search">
            <span v-if="loading" class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
            <span v-else>Search</span>
          </button>
        </form>
      </div>
  
      <div class="container mt-4">
        <div v-if="loading" class="text-center">
          <span class="spinner-border spinner-border-lg" role="status" aria-hidden="true"></span>
          <p>Searching...</p>
        </div>
  
        <table class="table mt-4" v-if="results.length">
          <thead>
            <tr>
              <th>Name</th>
              <th>Bedrooms</th>
              <th>Bathrooms</th>
              <th>Storeys</th>
              <th>Garages</th>
              <th>Price</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="property in results" :key="property.id">
              <td>{{ property.name }}</td>
              <td>{{ property.bedrooms }}</td>
              <td>{{ property.bathrooms }}</td>
              <td>{{ property.storeys }}</td>
              <td>{{ property.garages }}</td>
              <td>{{ property.price }}</td>
            </tr>
          </tbody>
        </table>
  
        <p v-else-if="!loading" class="text-center">No results found.</p>
      </div>
    </div>
  </template>
  
  <script>
  import axios from '@/axios';
  
  export default {
    data() {
      return {
        selectedParam: 'name', // Default selected parameter
        searchTerm: '',
        results: [],
        loading: false,
        searchParams: {
          name: '',
          bedrooms: '',
          bathrooms: '',
          storeys: '',
          garages: '',
          min_price: '',
          max_price: '',
          pageTitle: 'Property Search',
        },
      };
    },
    mounted() {
      document.title = this.searchParams.pageTitle;
    },
    methods: {
      search() {
        this.loading = true;
        axios.get('properties/search', { params: this.searchParams })
          .then(response => {
            this.results = response.data;
            this.loading = false;
            document.title = `Search Results - ${this.searchParams.pageTitle}`;
          })
          .catch(error => {
            console.error('Error:', error);
            this.loading = false;
          });
      },
    },
  };
  </script>
  
  <style scoped></style>
  