<template>
  <div class="hello">
    {{ this.Data.length }}
    <ul class="grid">
      <li v-for="item in this.Data" :key="item.name" class="item">
        <div class="heading">
          <div>
            ><span class="autor">{{ item.autor + " " }} </span>
            <span class="date">{{ item.date }}</span>
          </div>
          <!-- <span>{{ index }}</span> -->
        </div>
        <br />
        <span v-html="SplitProblems(item.content)"></span>
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
  box-shadow: rgba(17, 12, 46, 0.15) 0px 48px 100px 0px;
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
