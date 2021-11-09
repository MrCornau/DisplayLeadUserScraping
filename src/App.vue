<template>
  <div>
    <DisplayProblems
      v-if="this.Data[1].interestingcomments"
      v-bind:Heading="'Problem'"
      v-bind:Data="this.Data[1].interestingcomments"
    />
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
      Data: [],
      json: null, // passing array data into Vue
    };
  },
  async created() {
    const foobar = await this.getJson();
    foobar.forEach(async (item) => {
      let temp = await import("./json/" + item.replace("./", ""));
      this.Data.push(temp);
      console.log("test", this.Data[0].interestingcomments);
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
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
