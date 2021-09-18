<template>
  <div v-if="showBullets" class="image-gallery-bullets">
    <div class="image-gallery-bullets-container" role="navigation">
      <button
        type="button"
        class="image-gallery-bullet"
        :key="picture"
        v-for="(picture, index) in pictures"
        :class="[currentIndex === index ? 'image-gallery-bullet--active' : '']"
        v-on:click="$emit('clickedBullet', index)"
      ></button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Bullets",
  props: {
    pictures: { type: Array },
    currentIndex: { type: Number },
    checkboxes: { type: Array },
  },

  computed: {
    showBullets() {
      return this.checkboxes.includes("bullets");
    },
  },

  emits: ["clickedBullet"],
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.image-gallery-bullets {
  bottom: 20px;
  left: 0;
  margin: 0 auto;
  position: absolute;
  right: 0;
  width: 80%;
  z-index: 4;
}

.image-gallery-bullets-container {
  margin: 0;
  padding: 0;
  text-align: center;
}

.image-gallery-bullet {
  appearance: none;
  background-color: transparent;
  border: 1px solid #fff;
  border-radius: 50%;
  box-shadow: 0 2px 2px #1a1a1a;
  cursor: pointer;
  display: inline-block;
  margin: 0 5px;
  outline: none;
  padding: 5px;
  transition: all 0.2s ease-out;
}

.image-gallery-bullet--active {
  transform: scale(1.2);
  border: 1px solid #fff;
  background: #fff;
}
</style>
