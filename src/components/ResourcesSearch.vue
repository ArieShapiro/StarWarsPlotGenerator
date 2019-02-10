<template>
  <section>

    <input v-model="searchInput" type="text" placeholder="Resource name...">
    
    <button
      class="waves-effect btn light-blue darken-1"
      @click.prevent="onSearch"
    ><i class="fas fa-search"></i> Search For the Resource</button>

    <h2>{{ status }}</h2>

    <ul>
      <li class="card blue-grey darken-1" v-for="(result,idx) in results" :key="idx">
        <p class="card-content white-text">
          <b>Name:</b>
          {{ result.name }},
          <b>Type:</b>
          {{ result.type }}
          <button
            class="waves-effect btn light-blue darken-1 right"
            @click.prevent="onAddResource(idx)"
          ><i class="fas fa-plus"></i> Add</button>
        </p>
      </li>
    </ul>

  </section>
</template>

<script>
import axios from "axios";
import { bus } from "../main.js";

export default {
  name: "ResourcesSearch",
  data() {
    return {
      searchInput: "",
      results: [],
      status: ""
    };
  },
  methods: {
    onSearch() {
      this.results = [];
      this.status = "Checking...";
      //Search request to people API 
      axios
        .get(`https://swapi.co/api/people/?search=${this.searchInput}`)
        .then(res => {
          //If there are results, push each one to the this.results, and they reactively will render
          //on the view
          if (res.data.count) {
            this.status = "Search Results:";
            res.data.results.forEach(result => {
              let obj = { name: result.name, type: "person" };
              this.results.push(obj);
            });
          }
          //Search request to planets API
          axios
            .get(`https://swapi.co/api/planets/?search=${this.searchInput}`)
            .then(res => {
              //If there are results, push each one to the this.results, and they reactively will render
              //on the view
              if (res.data.count) {
                this.status = "Search Results:";
                res.data.results.forEach(result => {
                  let obj = { name: result.name, type: "planet" };
                  this.results.push(obj);
                });
              }
            });
          if (this.results.length === 0) this.status = "No results found";
        });
    },
    onAddResource(idx) {
      //Emits event and sends data with the chosen resource, that will be added to the plot
      bus.$emit("onAddResource", this.results[idx]);
    }
  },
};
</script>

<style scoped>
h2 {
  font-size: 1.5em;
}
p button {
  position: relative;
  bottom: 7px;
}
</style>
