<template>
  <section class="container">

    <form @submit.prevent="onSubmitForm">

      <input v-model="submitionData.title" type="text" placeholder="Title..." required>
      
      <input v-model="submitionData.description" type="text" placeholder="Description..." required>

      <ResourcesSearch/>

      <AddedResources :resources="submitionData.resources"/>

      <input  class="waves-effect btn light-blue darken-1" type="submit" value="Create">

      <button  class="waves-effect btn red darken-1" @click="onCancel">Cancel</button>

    </form>

  </section>
</template>

<script>
import ResourcesSearch from './ResourcesSearch';
import AddedResources from './AddedResources';
import { bus } from "../main.js";

export default {
  name: "NewPlotForm",
  components: {
    ResourcesSearch,
    AddedResources
  },
  data() {
    return {
      planet: '',
      person: '',
      submitionData: {
        title: "",
        description: "",
        resources: {
          planets: [],
          people: []
        }
      }
    };
  },
  methods: {
    onSubmitForm() {
        bus.$emit("onSubmitForm", this.submitionData);
    },
    onCancel() {
      bus.$emit("onCancel");
    },
  },
  created() {
    bus.$on('onAddResource', data => {
      if (data.type === 'planet') {
        this.submitionData.resources.planets.push(data);
      } else {
        this.submitionData.resources.people.push(data);
      }
    });
    bus.$on('onDeletePerson', idx => {
      this.submitionData.resources.people.splice(idx, 1);
    });
    bus.$on('onDeletePlanet', idx => {
      this.submitionData.resources.planets.splice(idx, 1);
    });
  }
};
</script>

<style scoped>
button {
  margin: 2%;
}
form {
  margin-top: 5%;
}
</style>
