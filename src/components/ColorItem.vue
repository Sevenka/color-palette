<template>
  <div class="color-item">
    <div class="color-item__preview" :style="{backgroundColor: `#${hex}`}"></div>
    <div class="color-item__description">
      <p class="color-item__title">{{ title }}</p>
      <p class="color-item__hex">{{ hex }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    hex: String,
    tags: Array
  },
  computed: {
    title() {
      return this.tags.reduce(
        (acc, currentValue) => `${acc} ${currentValue.name}`,
        ""
      );
    }
  }
};
</script>

<style lang="scss">
.color-item {
  display: flex;
  flex-basis: 300px;
  align-items: center;
  margin-bottom: 20px;
  margin-left: 10px;
  padding: 10px;
  border: 1px solid #bbb;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.19), 0 6px 6px rgba(0, 0, 0, 0.23);

  &__preview {
    width: 50px;
    height: 50px;
    margin-right: 10px;
  }
}
</style>