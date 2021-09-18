<template>
  <div
    class="image-gallery-slide"
    ref="input"
    :key="picture"
    v-for="(picture, index) in pictures"
    :style="{
      transform: 'translate3d(' + picture.value + '%, 0px, 0px)',
      transition: 'all ' + slideInterval + '' + 'ms ease-out 0s',
    }"
    :class="[currentIndex === index ? 'index-style' : '']"
  >
    <div class="index">
      <img
        class="image-gallery-image"
        :src="picture.src"
        :alt="picture.description"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: "CurrentImage",
  props: {
    pictures: Array,
    currentIndex: Number,
    slideInterval: Number,
  },

  computed: {
    style() {
      return { transition: `all 450ms ease-out 0s` };
    },
  },

  created() {
    window.addEventListener("resize", this.myEventHandler);
  },
  unmounted() {
    window.removeEventListener("resize", this.myEventHandler);
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.image-gallery-slide {
  left: 0;
  position: absolute;
  top: 0;
  width: 100%;
}

.image-gallery-image {
  max-height: calc(100vh - 80px);
  width: 100%;
  object-fit: contain;
}

.index-style {
  position: relative;
}
</style>
