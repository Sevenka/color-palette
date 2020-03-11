<template>
  <div id="app">
    <div class="color-items">
      <ColorItem
        v-for="colorItem in colorItems"
        :key="colorItem.id"
        :hex="colorItem.hex"
        :tags="colorItem.tags"
      />
    </div>
  </div>
</template>

<script>
import ColorItem from "./components/ColorItem";

export default {
  name: "App",
  components: {
    ColorItem
  },
  data() {
    return {
      colorItems: []
    };
  },
  created() {
    this.getColorItems();
  },
  methods: {
    getColorItems() {
      fetch("http://www.colr.org/json/colors/random/30")
        .then(response => response.json())
        .then(data => {
          this.colorItems = data.colors;
        })
        .catch(error => console.error(error));
    }
  }
};
</script>

<style lang="scss">
.color-items {
  display: flex;
  flex-wrap: wrap;
}
</style>
