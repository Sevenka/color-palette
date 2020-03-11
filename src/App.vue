<template>
  <div id="app">
    <div class="container">
      <input
        v-model="searchQuery"
        class="search-field"
        placeholder="Search color (by name, hex)..."
        type="text"
      />
      <div class="color-items">
        <ColorItem
          v-for="colorItem in filteredColorItems"
          :key="colorItem.id"
          :hex="colorItem.hex"
          :title="colorItem.title"
        />
        <p v-show="!filteredColorItems.length">No items</p>
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
      colorItems: [],
      loading: false
    };
  },
  computed: {
    filteredColorItems() {
      return this.colorItems.filter(item => {
        const transformedQuery = this.searchQuery.toLowerCase();
        return (
          item.hex.includes(transformedQuery) ||
          item.title.includes(transformedQuery)
        );
      });
    }
  },
  created() {
    this.getColorItems();
  },
  methods: {
    getColorItems() {
      this.loading = true;
      fetch("http://www.colr.org/json/colors/random/30")
        .then(response => response.json())
        .then(data => {
          this.loading = false;
          this.colorItems = data.colors.reduce((acc, currentItem) => {
            const { hex, id, tags } = currentItem;
            const title = tags.reduce(
              (acc, currentValue) => `${acc} ${currentValue.name}`,
              ""
            );
            if (!hex && !title) {
              return acc;
            } else {
              return [
                ...acc,
                {
                  hex,
                  id,
                  title
                }
              ];
            }
          }, []);
        })
        .catch(error => console.error(error));
    }
  }
};
</script>

<style lang="scss">
@import "./scss/helpers";

body {
  background-color: #f5f6f8;
}
.container {
  display: flex;
  flex-direction: column;
  max-width: 1280px;
  margin: 0 auto;
  padding: $base-size;
}
.search-field {
  margin-bottom: $base-size * 3;
  padding: $base-size;
  border: 1px solid #ccc;
  border-radius: $base-size / 2;
}
.color-items {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin-left: -$base-size * 3;
}
</style>
