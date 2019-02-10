<template>
  <section>
    
    <h2
      v-if="resources.planets.length === 0 && 
                resources.people.length === 0"
    >No Resources added</h2>

    <h2 v-else>Added Resources:</h2>

    <ul>
      <li class="card blue-grey darken-1" v-for="(planet, idx) in resources.planets" :key="idx">
        <p class="card-content white-text">
          <b>Name:</b>
          {{ planet.name }},
          <b>Type:</b>
          {{ planet.type }}
          <button
            class="waves-effect btn red darken-1 right"
            @click.prevent="onDeletePlanet(idx)"
          >
            <i class="fas fa-trash-alt"></i>
          </button>
        </p>
      </li>
    </ul>

    <ul>
      <li class="card blue-grey darken-1" v-for="(person, idx) in resources.people" :key="idx">
        <p class="card-content white-text">
          <b>Name:</b>
          {{ person.name }},
          <b>Type:</b>
          {{ person.type }}
          <button
            class="waves-effect btn red darken-1 right center"
            @click.prevent="onDeletePerson(idx)"
          >
            <i class="fas fa-trash-alt"></i>
          </button>
        </p>
      </li>
    </ul>
  </section>
</template>

<script>
import { bus } from "../main.js";

export default {
  name: "AddesResources",
  props: ["resources"],
  methods: {
    onDeletePlanet(idx) {
      bus.$emit("onDeletePlanet", idx);
    },
    onDeletePerson(idx) {
      bus.$emit("onDeletePerson", idx);
    }
  }
};
</script>

<style scoped>
button {
  margin: 2%;
  position: relative;
  bottom: 23px;
}
h2 {
  font-size: 1.5em;
}
</style>
