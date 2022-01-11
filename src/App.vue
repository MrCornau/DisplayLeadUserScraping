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
            v-for="(item, index) in this.Files"
            :key="item.name"
            class="list-item"
          >
            <a
              v-on:click="counter3 = index"
              v-bind:class="[{ listItemSelected: counter3 === index }]"
            >
              <h1>{{ item }}</h1>
            </a>
          </div>
        </div>
        <div class="switch">
          <div
            v-for="(item, index) in this.Data[counter3]"
            :key="item.name"
            class="list-item"
          >
            <a
              v-on:click="counter = index"
              v-bind:class="[{ listItemSelected: counter === index }]"
            >
              <h1>{{ this.Data[counter3][index].name }}</h1>
            </a>
          </div>
        </div>
      </div>
    </div>
    <div>
      <DisplayProblems
        v-if="
          pagecount == 1 && this.Data[counter3][counter].interestingcomments
        "
        v-bind:Heading="'Problem'"
        v-bind:Data="this.Data[counter3][counter].interestingcomments"
        v-bind:SaveButton="true"
        v-bind:IsDelete="false"
        v-bind:Procedure="this.Data[counter3][counter].Procedure"
        v-bind:SearchParam="this.Data[counter3][counter].SearchParam"
        v-bind:FileName="this.Data[counter3][counter].name"
        @add="postComment"
      >
      </DisplayProblems>
    </div>

    <DisplayProblems
      v-if="pagecount == 0 && this.interestingComments[counter2]"
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
      counter3: 0,
      test: true,
      Data: [],
      Files: [],
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
    const bafoo = await this.getFolder();
    bafoo.forEach(async (item) => {
      this.Files.push(item);
    });

    foobar[1].forEach(async (item, i) => {
      this.Data[i] = [];
      item.forEach(async (item) => {
        let temp = await import("./json/" + item.replace("./", ""));
        this.Data[i].push(temp);
      });
    });

    try {
      const res = await axios.get(this.url[0].url);
      const res2 = await axios.get(this.url[1].url);
      this.interestingComments = [res.data, res2.data];
    } catch (e) {
      console.log("here", e);
    }
  },
  methods: {
    async postComment(item, way) {
      await axios.post(this.url[way].url, {
        autor: item.autor,
        date: item.date,
        content: item.content,
        link: item.link,
        origin: item.origin,
        suborigin: item.suborigin,
        selector: item.Selector,
        selectorShort: item.selectorShort,
        MarkedSent: item.MarkedSent,
        sortedWord: item.sortedWord,
        Identifyer: item.Identifyer,
        identifyer: item.identifyer,
        year: item.year,
        tag: "",
        rating: 3,
        heading: "",
        description: "",
      });
      const res = await axios.get(this.url[way].url);
      this.interestingComments[way] = res.data;
    },
    async deleteComment(item, way) {
      await axios.delete(this.url[way].url + "/" + item.id);
      const res = await axios.get(this.url[way].url);
      this.interestingComments[way] = res.data;
    },
    getFolder() {
      let req = require.context("@/json", true, /^\.\//);
      let arr = req.keys();
      let map2 = arr.map((x) => x.split("/")[1]);

      // console.log([...new Set(map2)]);
      return [...new Set(map2)];
    },
    getJson() {
      const files = require.context("@/json", true, /^.*json$/);
      files.keys();
      let arr2 = files.keys();
      let map3 = arr2.map((x) => x.split("/")[1]);
      let newArr = [...new Set(map3)];
      let retArr = [];
      newArr.forEach(async (item, i) => {
        retArr[i] = [];
        arr2.forEach(async (item2) => {
          if (item2.includes(item)) {
            // console.log(item, item2);
            retArr[i].push(item2);
          }
        });
      });
      // console.log(retArr);
      return [files.keys(), retArr];
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
  overflow: scroll;
  white-space: nowrap;
}
.list-item {
  margin-left: 1%;
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
  margin-top: 240px;
}
body {
  margin-left: 0px !important;
}

ul {
  list-style-type: none;
  padding: 0;
}
</style>

"autor": "ntoporcov", "date": "2019-01-18 06:21:37", "content": "I created a web
app for photographers who use Google Drive to send photos to clients but wish
they had a better looking page to send to clients.", "link":
"https://www.reddit.com/r/Nikon/comments/ah7lvu/i_created_a_web_app_for_photographers_who_use/",
"origin": "Reddit", "suborigin": "nikon", "result": true, "Selector": "i
created", "selectorShort": "create", "MarkedSent": "-----> i !!! -----> created
!!! a web app for photographers who use google drive to send photos to clients
but wish they had a better looking page to send to clients.", "sortedWord":
"None", "Identifyer": null, "identifyer": 1248, "year": "2019"
