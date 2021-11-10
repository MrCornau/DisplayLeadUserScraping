<template>
  <div class="hello">
    {{ this.Data.length }}
    <ul class="grid">
      <li v-for="item in this.Data" :key="item.name" class="item">
        <div class="heading">
          <span class="autor">{{ item.autor + " " }} </span>
          <span class="date">{{ item.date }}</span>
        </div>
        <br />
        <span v-html="SplitProblems(item.content)"></span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "DisplayProblems",
  props: {
    Data: JSON,
    Heading: String,
  },
  methods: {
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
.grid {
  column-count: 2;
  column-gap: 20;
  padding: 10px;
  text-align: left;
}
.autor {
  font-weight: bold;
}
.date {
  font-style: italic;
  color: blue;
}

.item {
  display: inline-block; /* important to wrap notes not content */
  width: 98%;
  background-color: #ffffff;
  margin-top: 20px;
  padding: 1%;
  border-radius: 5px;
  box-shadow: rgba(17, 12, 46, 0.15) 0px 48px 100px 0px;
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
