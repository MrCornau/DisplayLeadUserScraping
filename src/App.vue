<template>
  <div>
    <div class="switch">
      <div class="list-item">
        <a
          v-on:click="pagecount = 0"
          v-bind:class="[{ listItemSelected: pagecount === 0 }]"
          ><h1>Selected Comments</h1></a
        >
      </div>
      <div class="list-item">
        <a
          v-on:click="pagecount = 1"
          v-bind:class="[{ listItemSelected: pagecount === 1 }]"
          ><h1>RawData</h1></a
        >
      </div>
    </div>
    <div v-if="pagecount == 1">
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
          v-bind:SaveButton="true"
          v-bind:IsDelete="false"
        />
      </div>
    </div>
    <div>
      <div v-if="pagecount == 0">
        <DisplayProblems
          v-if="this.interestingComments"
          v-bind:Heading="'Problem'"
          v-bind:Data="this.interestingComments"
          v-bind:SaveButton="false"
          v-bind:IsDelete="true"
        />
      </div>
    </div>
  </div>
</template>

<script>
import DisplayProblems from "./components/DisplayProblems.vue";

import axios from "axios";
const baseurl = "https://fakerestnlp.herokuapp.com/positiveComments";

export default {
  name: "App",
  components: {
    DisplayProblems,
  },
  data: function () {
    return {
      interestingComments: [],
      pagecount: 0,
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
    });

    try {
      const res = await axios.get(baseurl);
      this.interestingComments = res.data;
    } catch (e) {
      console.log(e);
    }
  },
  methods: {
    async postComment() {
      await axios.post(baseurl, {
        autor: "Jmeynn",
        selftext: "hdjjdjdj",
      });
    },
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
