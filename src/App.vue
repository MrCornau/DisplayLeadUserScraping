<template>
  <div>
    <div class="switch">
      <div
        v-for="(item, index) in this.Data"
        :key="item.name"
        class="list-item"
      >
        <a
          v-on:click="counter = index"
          v-bind:class="[{ listItemSelected: counter === index }]"
        >
          <h1>{{ this.Data[index].name }}</h1>
        </a>
      </div>
    </div>

    <div>
      <DisplayProblems
        v-if="this.Data[counter].interestingcomments"
        v-bind:Heading="'Problem'"
        v-bind:Data="this.Data[counter].interestingcomments"
      />
    </div>
  </div>
</template>

<script>
import DisplayProblems from "./components/DisplayProblems.vue";

export default {
  name: "App",
  components: {
    DisplayProblems,
  },
  data: function () {
    return {
      counter: 0,
      test: true,
      Data: [],
      json: null, // passing array data into Vue
    };
  },
  async created() {
    const foobar = await this.getJson();
    foobar.forEach(async (item) => {
      let temp = await import("./json/" + item.replace("./", ""));
      this.Data.push(temp);
      console.log("test", this.Data);
    });
  },
  methods: {
    getJson() {
      const files = require.context("@/json", true, /^.*json$/);
      files.keys();
      console.log(files.keys());
      return files.keys();
    },
  },
};
</script>

<style>
.switch {
  display: flex;
}
.list-item {
  margin: 1%;
  margin-right: 30px;
  cursor: pointer;
}

.listItemSelected {
  color: blue;
  text-decoration: underline blue;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

ul {
  list-style-type: none;
  padding: 0;
}
</style>
