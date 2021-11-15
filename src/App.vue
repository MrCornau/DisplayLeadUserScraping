<template>
  <div>
    <div class="header">
      <div class="switch">
        <div class="list-item">
          <a
            v-on:click="pagecount = 0"
            v-bind:class="[{ HeaderItemSelected: pagecount === 0 }]"
            ><h1>Selected Comments</h1></a
          >
        </div>
        <div class="list-item">
          <a
            v-on:click="pagecount = 1"
            v-bind:class="[{ HeaderItemSelected: pagecount === 1 }]"
            ><h1>RawData</h1></a
          >
        </div>
      </div>
      <div v-if="pagecount == 0">
        <div class="switch">
          <div
            v-for="(item, index) in this.url"
            :key="item.id"
            class="list-item"
          >
            <a
              v-on:click="counter2 = index"
              v-bind:class="[{ listItemSelected: counter2 === index }]"
            >
              <h1>{{ item.name }}</h1>
            </a>
          </div>
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
      </div>
    </div>
    <div>
      <DisplayProblems
        v-if="this.Data[counter].interestingcomments"
        v-bind:Heading="'Problem'"
        v-bind:Data="this.Data[counter].interestingcomments"
        v-bind:SaveButton="true"
        v-bind:IsDelete="false"
        @add="postComment"
      />
    </div>

    <DisplayProblems
      v-if="this.interestingComments[counter2]"
      v-bind:Heading="'Problem'"
      v-bind:Data="this.interestingComments[counter2]"
      v-bind:SaveButton="false"
      v-bind:IsDelete="true"
      v-bind:isLead="counter2"
      @delete="deleteComment"
    />
  </div>
</template>

<script>
import DisplayProblems from "./components/DisplayProblems.vue";

import axios from "axios";

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
      counter2: 0,
      test: true,
      Data: [],
      json: null, // passing array data into Vue
      url: [
        {
          url: "https://fakerestnlp.herokuapp.com/positiveComments",
          name: "Lead User",
        },
        {
          url: "https://fakerestnlp.herokuapp.com/uselesComments",
          name: "Bad",
        },
      ],
    };
  },
  async created() {
    const foobar = await this.getJson();
    foobar.forEach(async (item) => {
      let temp = await import("./json/" + item.replace("./", ""));
      this.Data.push(temp);
    });

    try {
      const res = await axios.get(this.url[0].url);
      const res2 = await axios.get(this.url[1].url);
      this.interestingComments = [res.data, res2.data];
    } catch (e) {
      console.log(e);
    }
  },
  methods: {
    async postComment(item, way) {
      console.log("realtest", item, way);
      await axios.post(this.url[way].url, {
        autor: item.autor,
        selftext: item.selftext,
        title: item.title,
        date: item.date,
        content: item.content,
        link: item.link,
      });
      const res = await axios.get(this.url[way].url);
      this.interestingComments[way] = res.data;
    },
    async deleteComment(item, way) {
      await axios.delete(this.url[way].url + "/" + item.id);
      const res = await axios.get(this.url[way].url);
      this.interestingComments[way] = res.data;
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
.header {
  position: fixed;
  top: 0;
  width: 100%;
  background-color: whitesmoke;
  box-shadow: rgba(17, 12, 46, 0.15) 0px 48px 100px 0px;
}

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

.HeaderItemSelected {
  color: blue;
  text-decoration: underline blue;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 290px;
}
body {
  margin-left: 0px !important;
}

ul {
  list-style-type: none;
  padding: 0;
}
</style>
