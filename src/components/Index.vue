<template>
  <div v-if="showIndex" class="image-gallery-index">
    <span> {{ showCurrentIndex }} </span>
  </div>
</template>

<script>
export default {
  name: "Index",
  props: {
    pictures: { type: Array },
    currentIndex: { type: Number },
    checkboxes: { type: Array },
  },

  computed: {
    showCurrentIndex() {
      return `${this.currentIndex + 1} / ${this.pictures.length}`;
    },
    showIndex() {
      return this.checkboxes.includes("index");
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.image-gallery-index {
  background: rgba(0, 0, 0, 0.4);
  color: #fff;
  line-height: 1;
  padding: 10px 20px;
  position: absolute;
  right: 0;
  top: 0;
  z-index: 4;
}
</style>
