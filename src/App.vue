<template>
  <section class="nav-wrapper center">

    <nav>
      <header class="brand-logo center">Star Wars Plot Generator</header>
    </nav>

    <button
      class="waves-effect btn light-blue darken-1"
      v-if="!isFormOpened"
      @click="onCreateNewPlot"
    >
      Create New Plot
      <i class="fas fa-pen"></i>
    </button>

    <NewPlotForm v-if="isFormOpened"/>

    <ul class="container">
      <li class="center card blue-grey darken-1" v-for="(plot, idx) in plots" :key="idx">
        <Plot :plot="plot"/>
        <button class="waves-effect btn red darken-1" @click="onDeletePlot(idx)">
          <i class="fas fa-trash-alt"></i>
        </button>
      </li>
    </ul>

  </section>
</template>

<script>
import Plot from "./components/Plot";
import NewPlotForm from "./components/NewPlotForm";
import axios from "axios";
import { bus } from "./main.js";

export default {
  name: "app",
  components: {
    Plot,
    NewPlotForm
  },
  data() {
    return {
      isFormOpened: true,
      plots: []
    };
  },
  methods: {
    onCreateNewPlot() {
      this.isFormOpened = !this.isFormOpened;
    },
    onDeletePlot(idx) {
      this.plots.splice(idx, 1);
    }
  },
  created() {
    //I use the 'EventBus' in order to emit events from components. It's in some way more 
    //comfortable than the regular event emitter from child to parent, because here you don't
    //need to think about the exact ralations of the components.You just 'catch' the event where ever
    //you want

    //The two events below come from NewPlotForm component
    bus.$on("onSubmitForm", data => {
      this.isFormOpened = !this.isFormOpened;
      this.plots.push(data);
    });
    bus.$on("onCancel", () => {
      this.isFormOpened = !this.isFormOpened;
    });
  }
};
</script>

<style scoped>
/* Added some styles of my own in order to knock down some Framework rules, to make some
things look a little nicer */
button,
new-plot-form {
  margin-top: 5%;
}
i {
  margin-left: 15px;
}
li button i {
  position: relative;
  right: 7px;
}
li button {
  position: relative;
  bottom: 14px;
}
header {
  font-family: 'Monoton', cursive;
}
</style>
