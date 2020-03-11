<template>
  <div id="app">
    <div class="container">
      <input v-model="searchQuery" type="text" />
      <div class="color-items">
        <ColorItem
          v-for="colorItem in filteredColorItems"
          :key="colorItem.id"
          :hex="colorItem.hex"
          :title="colorItem.title"
        />
      </div>
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
      searchQuery: "",
      colorItems: []
    };
  },
  computed: {
    filteredColorItems() {
      return this.colorItems.filter(
        item =>
          item.hex.includes(this.searchQuery) ||
          item.title.includes(this.searchQuery)
      );
    }
  },
  created() {
    this.getColorItems();
  },
  methods: {
    getColorItems() {
      fetch("http://www.colr.org/json/colors/random/30")
        .then(response => response.json())
        .then(data => {
          this.colorItems = data.colors.map(item => {
            const { hex, id } = item;
            const title = item.tags.reduce(
              (acc, currentValue) => `${acc} ${currentValue.name}`,
              ""
            );
            return {
              hex,
              id,
              title
            };
          });
        })
        .catch(error => console.error(error));
    }
  }
};
</script>

<style lang="scss">
.container {
  max-width: 1280px;
  margin: 0 auto;
  padding: 10px;
}
.color-items {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
</style>
