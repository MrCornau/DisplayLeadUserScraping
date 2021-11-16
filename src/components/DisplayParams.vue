<template>
  <div class="bigContainer">
    <div>
      <h3>Search Params</h3>
      <div
        v-for="item in this.SearchParam"
        :key="item.id"
        v-bind:class="[{ isselector: item.display === 'selector' }]"
      >
        <div class="container ParamContainer">
          <div v-for="param in item" :key="param.id" class="isselector">
            {{ param }}
          </div>
        </div>
      </div>
    </div>

    <div>
      <h3>Procedure</h3>
      <div class="container">
        <div
          v-for="item in this.Procedure"
          :key="item.id"
          v-bind:class="[{ isselector: item.display === 'selector' }]"
        >
          {{ item.content }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "DisplayParams",
  props: {
    Procedure: Array,
    SearchParam: Array,
  },
  data: function () {
    return {};
  },
  methods: {},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.isselector {
  color: blue;
  background-color: rgba(172, 209, 244, 0.2);
  font-weight: bold;
  border-radius: 3px;
  width: auto;
  height: auto;
  padding: 2px 2px 2px 2px;
  margin-left: 2px;
  margin-right: 2px;
}

.container {
  display: flex;
  gap: 8px;
  margin-bottom: 18px;
}

h3 {
  display: flex;

  margin-top: 32px;
}

.bigContainer {
  display: flex;
  gap: 50px;
  margin-left: 1%;
}
</style>
