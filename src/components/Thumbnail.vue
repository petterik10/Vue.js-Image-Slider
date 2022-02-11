<template>
  <div
    class="image-gallery-thumbnails-wrapper bottom thumbnails-swipe-horizontal"
  >
    <div :style="style" class="image-gallery-thumbnails">
      <div
        :style="{
          transform: `translate3d(-${playTransitionLeft}px, -${playTransitionCenter}px, 0px)`,
          transition: 'all 450ms ease-out 0s',
        }"
        class="image-gallery-thumbnails-container"
      >
        <button
          type="button"
          class="image-gallery-thumbnail"
          :key="picture"
          v-for="(picture, index) in pictures"
          :class="[
            currentIndex === index ? 'image-gallery-thumbnail--active' : '',
          ]"
          v-on:click="$emit('clickedBullet', index)"
        >
          <span class="image-gallery-thumbnail-inner">
            <img
              class="image-gallery-thumbnail-image"
              :src="picture.src"
              :alt="picture.description"
            />
          </span>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Thumbnail",
  props: {
    pictures: { type: Array },
    currentIndex: { type: Number },
    thumbnailPosition: { type: String },
    thumbnailHeight: { type: Number },
    thumbnailWidth: { type: Number },
    maxHeight: { type: Number },
  },

  methods: {
    setTransition(num, directionPossibility1, directionPossibility2) {
      const absoluteValue = num / 11;
      const indexedValue = this.currentIndex + 1;
      if (window.matchMedia("(min-width: 1740px)").matches) {
        return 0;
      } else if (
        this.thumbnailPosition === directionPossibility1 ||
        this.thumbnailPosition === directionPossibility2
      ) {
        const transitionValue =
          this.currentIndex === 0 ? 0 : absoluteValue * indexedValue;
        return transitionValue;
      } else {
        return 0;
      }
    },
  },
  computed: {
    style() {
      return this.thumbnailPosition === "Right" ||
        this.thumbnailPosition === "Left"
        ? { height: `${this.thumbnailHeight}px` }
        : null;
    },
    playTransitionLeft() {
      return this.setTransition(this.thumbnailWidth, "Top", "Bottom");
    },
    playTransitionCenter() {
      return this.setTransition(this.maxHeight, "Right", "Left");
    },
  },

  emits: ["clickedBullet"],
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.image-gallery-thumbnails-wrapper {
  position: relative;
}

.thumbnails-swipe-horizontal {
  touch-action: pan-y;
}

.image-gallery-thumbnails {
  overflow: hidden;
  padding: 5px 0;
}

.image-gallery-thumbnails-container {
  cursor: pointer;
  text-align: center;
  white-space: nowrap;
}

.image-gallery-thumbnail {
  margin-left: 2px;
  display: inline-block;
  border: 4px solid transparent;
  transition: border 0.3s ease-out;
  width: 100px;
  background: transparent;
  padding: 0;

  &:hover {
    outline: none;
    border: 4px solid #337ab7;
  }
}

.image-gallery-thumbnail-inner {
  display: block;
  position: relative;
}

.image-gallery-thumbnail-image {
  vertical-align: middle;
  width: 100%;
  line-height: 0;
}

.image-gallery-thumbnail--active {
  outline: none;
  border: 4px solid #337ab7;
}

.leftVetical {
  height: 321px;
}
</style>
