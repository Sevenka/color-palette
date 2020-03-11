<template>
  <div @click="copyColor" class="color-item">
    <div class="preview" :style="{backgroundColor: hexCode}"></div>
    <div class="description">
      <p class="title">{{ title }}</p>
      <p class="hex">{{ hexCode }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    hex: String,
    title: String
  },
  computed: {
    hexCode() {
      return `#${this.hex}`;
    }
  },
  methods: {
    copyColor() {
      navigator.clipboard.writeText(this.hexCode).then(
        () => {
          alert("Copying a hexcode to clipboard was successful!");
        },
        err => {
          alert("Could not copy text: ", err);
        }
      );
    }
  }
};
</script>

<style lang="scss">
@import "../scss/helpers";

.color-item {
  box-sizing: border-box;
  display: flex;
  flex-basis: 30%;
  align-items: center;
  margin-bottom: $base-size*3;
  margin-left: $base-size*3;
  padding: $base-size;
  border: 1px solid #bbb;
  background-color: #fff;
  box-shadow: 0 $base-size $base-size*2 rgba(0, 0, 0, 0.19), 0 $base-size $base-size rgba(0, 0, 0, 0.23);
  transition: all .2s ease-in-out;

  &:hover {
    cursor: pointer;
    transform: scale(1.05);
  }
}
.preview {
  width: $base-size*5;
  height: $base-size*5;
  margin-right: $base-size;
  border-radius: 50%;
}
</style>