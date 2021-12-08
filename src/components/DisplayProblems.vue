<template>
  <div class="hello">
    <h2 class="itemcount">{{ this.Data.length }} Items</h2>

    <DisplayParams
      v-if="this.Procedure"
      v-bind:Procedure="this.Procedure"
      v-bind:SearchParam="this.SearchParam"
    ></DisplayParams>
    <ul class="grid">
      <li v-for="item in this.Data" :key="item.name" class="item">
        <div class="heading">
          <div>
            ><span class="autor">{{ item.autor + " " }} </span>
            <span class="date">{{ item.date }}</span>
          </div>
          <div v-if="item.origin" class="date">
            {{ item.origin }}/{{ item.suborigin }}
          </div>
          <!-- <span>{{ index }}</span> -->
        </div>
        <br />
        <span v-html="SplitProblems(item.MarkedSent)"></span>
        <div class="ButtonFlex">
          <a
            v-if="item.link"
            class="button"
            v-bind:href="item.link"
            target="_blank"
          >
            <div class="button-font">Check out post</div>

            <span class="button-seperator"></span>
            <img
              class="button-icon"
              src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAwCAYAAABXAvmHAAAAAXNSR0IArs4c6QAAAR1JREFUaEPtl8ENAjEMBO1OeEHKoBOgMq4TKON+0IlRhPLipIsTr0Mk521HO7uWnDBNfnhy/RQAoxOMBCKBTgdihDoN7G6PBKosTHKmlZ9VtcoifAJHWYjpQkw3WnlR6tstxwIU8UUGAAIHkMdG6PFjoTEEDiArT3IloTsSAgvgAIEHAEP4AAAh/ABAEL4AAAh/AGOIMQCGEHqAk8jufu8tUCy7/wTIBlRCBEDvtGz2V7r/DWrUMXonjQEwEj8mAUPx/gDG4n0BAOL9AEDifQCA4vEAYPFYgCQHEnoh/8NYgK0Xp2LD1u5X/CIrYwQQj0+g2JjHaeV3rauaOnwCGjUNtQHQYJppSyRgamfDZZFAg2mmLZGAqZ0Nl02fwAeQR4IxQfl2sQAAAABJRU5ErkJggg=="
            />
          </a>
          <div class="ButtonFlex__Container">
            <a
              v-if="SaveButton"
              class="button"
              v-on:click="toggle"
              @click="postit(item, 0)"
            >
              <div class="button-font">Lead User</div>
            </a>
            <a
              v-if="SaveButton"
              class="button"
              v-on:click="toggle"
              @click="postit(item, 1)"
            >
              <div class="button-font">Bad User</div>
            </a>

            <a
              v-if="IsDelete"
              class="button"
              v-on:click="toggle"
              @click="deleteComment(item, this.isLead)"
            >
              <div class="button-font">Delete</div>
            </a>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import DisplayParams from "/src/components/DisplayParams.vue";

export default {
  name: "DisplayProblems",
  props: {
    SaveButton: Boolean,
    IsDelete: Boolean,
    Data: JSON,
    Heading: String,
    isLead: Number,
    method: { type: Function },
    Procedure: Array,
    SearchParam: Array,
    FileName: String,
  },
  components: {
    DisplayParams,
  },
  data: function () {
    return {
      interestingComments: [],
      pagecount: 0,
      counter: 0,
      test: true,
      UpdatedData: this.Data,
      json: null, // passing array data into Vue
    };
  },
  methods: {
    // async postComment(item) {
    //   await axios.post(baseurl, {
    //     autor: item.autor,
    //     selftext: item.selftext,
    //     title: item.title,
    //     date: item.date,
    //     content: item.content,
    //     link: item.link,
    //   });
    // },

    postit(item, way) {
      console.log(item, "test123");

      let origin = item.link.replace("https://www.", "").split("/")[0];

      console.log(origin, item.link);
      this.$emit("add", item, way, origin, this.FileName);
    },
    deleteComment(item, way) {
      this.$emit("delete", item, way);
    },
    toggle(event) {
      event.target.classList.toggle("SavedClass");
      console.log("test");
    },
    SplitProblems(string) {
      let checked = string.split(/\s+/);

      let newChecked = [];

      checked.forEach(function (item) {
        if (item == "----->") {
          newChecked.push(
            item.replace(
              "----->",
              '<span style="color:blue; background-color: rgba(172, 209, 244, 0.2); font-weight: bold; border-radius: 3px; width: auto;  height: auto; padding: 2px 2px 2px 2px; margin-left:2px;margin-right:2px">'
            )
          );
        } else if (item == "!!!") {
          newChecked.push(item.replace("!!!", "</span>"));
        } else {
          newChecked.push(item);
        }
      });

      return newChecked.join(" ");
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.itemcount {
  display: flex;
  margin-left: 1%;
}

.heading {
  display: flex;
  justify-content: space-between;
}

.ButtonFlex {
  display: flex;
  justify-content: space-between;
}

.ButtonFlex__Container {
  display: flex;
  gap: 8px;
}

.SavedClass {
  color: red !important;
}

.button {
  text-decoration: none;
  margin-top: 16px;
  padding: 4px 8px 4px 8px;
  display: flex;

  gap: 8px;
  align-items: center;
  background: rgba(172, 209, 244, 0.2);
  /* box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.05); */
  border-radius: 3px;
  cursor: pointer;
  width: -webkit-max-content;
  justify-content: center;
  color: blue;
  height: 20px;
}

.button:hover {
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
  border-style: 1px blue;
}

.button-icon {
  height: 24px;
}

.button-seperator {
  content: "";
  display: inline-block;
  background: blue;

  height: 100%;
  width: 0.5px;
  vertical-align: top;
}

.autor {
  font-weight: bold;
}
.date {
  font-style: italic;
  color: blue;
}

.grid {
  column-count: 2;
  column-gap: 20;
  padding: 10px;
  text-align: left;
}
.item {
  /* display: inline-block; important to wrap notes not content */
  width: 98%;
  background-color: #ffffff;
  margin-top: 20px;
  padding: 1%;
  border-radius: 5px;
  box-shadow: rgba(17, 12, 46, 0.05) 0px 48px 100px 0px;
}

.item:nth-child(3n + 1) {
  order: 1;
}
.item:nth-child(3n + 2) {
  order: 2;
}
.item:nth-child(3n) {
  order: 3;
}

/* .item{
    margin: 1%;
  width:45%;
 display:block;
  float:left;
  box-sizing: border-box;
  padding: 1%;
  background-color:#FFFFFF;
  border-radius: 5px;
  box-shadow: rgba(17, 12, 46, 0.15) 0px 48px 100px 0px;
}
.item:nth-child(3n + 0) { float: right; } */

.red {
  color: red;
}
h3 {
  margin: 40px 0 0;
}
h1 {
  text-align: left;
  margin: 1%;
  font-size: 50px;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
