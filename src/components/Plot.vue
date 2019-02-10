<template>
  <section class="center white-text">

    <h3 class="card-title">Titile:</h3>
    <p>{{ plot.title }}</p>

    <h3 class="card-title">Description:</h3>
    <p>{{ plot.description }}</p>

    <h3 class="card-title" v-if="plot.resources.planets.length === 0">No Planets added</h3>

    <h3 class="card-title" v-else>Planets:</h3>
    <ul>
      <li v-for="(planet, idx) in plot.resources.planets" :key="idx">
        <p>{{ planet.name }}</p>
      </li>
    </ul>

    <h3 class="card-title" v-if="plot.resources.people.length === 0">No People added</h3>

    <h3 class="card-title" v-else>People:</h3>
    <ul>
      <li v-for="(person, idx) in plot.resources.people" :key="idx">
        <p>{{ person.name }}</p>
      </li>
    </ul>

  </section>
</template>

<script>
export default {
  name: "Plot",
  props: ["plot"]
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  font-size: 1.5em;
  font-family: "Special Elite", cursive;
}
p {
  font-family: "Marck Script", cursive;
  font-size: 1.5em;
}
section {
  position: relative;
  top: 16px;
}
</style>
