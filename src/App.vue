<template>
  <Header />
  <section class="content">
    <div class="image-gallery app-image-gallery image-gallery-using-mouse">
      <div class="image-gallery-content bottom">
        <div :class="classObject" v-show="leftOrTop">
          <Thumbnail
            @clickedBullet="setClickedPicture"
            :pictures="pictures"
            :currentIndex="currentIndex"
            :thumbnailPosition="thumbnailPosition"
            :thumbnailHeight="thumbnailHeight"
            :thumbnailWidth="thumbnailWidth"
            :maxHeight="maxHeight"
          />
        </div>
        <div v-bind:class="left" class="image-gallery-slide-wrapper">
          <Button
            class="image-gallery-icon image-gallery-left-nav"
            @click="goToPrevSlide"
          >
            <font-awesome-icon :icon="['fas', 'angle-left']" />
          </Button>
          <Button
            class="image-gallery-icon image-gallery-right-nav"
            @click="goToNextSlide"
          >
            <font-awesome-icon :icon="['fas', 'angle-right']" />
          </Button>
          <div class="image-gallery-swipe">
            <div ref="slideRef" class="image-gallery-slides">
              <CurrentImage
                :pictures="pictures"
                :currentIndex="currentIndex"
                :slideInterval="slideInterval"
              />
            </div>
          </div>
          <Button
            class="image-gallery-icon image-gallery-play-button"
            @click="setPlayAndPause"
          >
            <font-awesome-icon :icon="['fas', isPlaying ? 'pause' : 'play']" />
          </Button>
          <div>
            <Bullets
              @clickedBullet="setClickedPicture"
              :pictures="pictures"
              :currentIndex="currentIndex"
              :checkboxes="checkboxes"
            />
          </div>
          <Index
            :currentIndex="currentIndex"
            :pictures="pictures"
            :checkboxes="checkboxes"
          />
        </div>

        <div v-bind:class="classObject" v-show="rightOrBottom">
          <Thumbnail
            @clickedBullet="setClickedPicture"
            :pictures="pictures"
            :currentIndex="currentIndex"
            :thumbnailPosition="thumbnailPosition"
            :thumbnailHeight="thumbnailHeight"
            :thumbnailWidth="thumbnailWidth"
            :maxHeight="maxHeight"
          />
        </div>
      </div>
    </div>

    <Settings
      @intervalTime="changePlayInterval"
      @setPlayingToFalse="setPlayingToFalse"
      @changeSlideInterval="changeSlideInterval"
      @changeThumbnailPosition="changeThumbnailPosition"
      :slideInterval="slideInterval"
      @update-checked="updateCheckBoxes"
    />
  </section>
</template>

<script>
import Header from "./components/Header.vue";
import CurrentImage from "./components/CurrentImage.vue";
import Button from "./components/Button.vue";
import Bullets from "./components/Bullets.vue";
import Thumbnail from "./components/Thumbnail.vue";
import Settings from "./components/Settings.vue";
import Index from "./components/Index.vue";

export default {
  name: "App",
  components: {
    CurrentImage,
    Button,
    Bullets,
    Thumbnail,
    Settings,
    Index,
    Header,
  },
  data() {
    return {
      pictures: [
        {
          value: 0,
          src: require("@/assets/ocean.jpg"),
          description: "Ocean view with mountain",
        },
        {
          value: 100,
          src: require("@/assets/town.jpg"),
          description: "bridge in old town",
        },
        {
          value: 200,
          src: require("@/assets/edinburg.jpg"),
          description: "Edinburg",
        },
        {
          value: 300,
          src: require("@/assets/sunset.jpg"),
          description: "Sunset on the beach",
        },
        {
          value: 400,
          src: require("@/assets/sunset2.jpg"),
          description: "Sunset",
        },
        {
          value: 500,
          src: require("@/assets/sunset3.jpg"),
          description: "Sunset",
        },
        {
          value: 600,
          src: require("@/assets/cottage-view.jpg"),
          description: "mountain behind cottage",
        },
        {
          value: 700,
          src: require("@/assets/view.jpg"),
          description: "City life at night",
        },
        {
          value: 800,
          src: require("@/assets/water.jpg"),
          description: "Glacier",
        },
        {
          value: 900,
          src: require("@/assets/forest.jpg"),
          description: "Forest view",
        },
        {
          value: 1000,
          src: require("@/assets/green.jpg"),
          description: "Grass",
        },
      ],
      data: [],
      currentIndex: 0,
      direction: "",
      playIntervalTime: 2000,
      isPlaying: false,
      intervalId: null,
      slideInterval: 450,
      thumbnailPosition: "Bottom",
      checkboxes: ["bullets", "index", "thumbnails"],
      thumbnailHeight: 920,
      thumbnailWidth: 300,
      maxHeight: 400,
      resizeObserver: null,
    };
  },

  methods: {
    setClickedPicture(index) {
      this.currentIndex = index;
      this.direction = "right";
      this.nextSlide(this.pictures, this.currentIndex);
    },

    updateCheckBoxes(values) {
      this.checkboxes = values;
    },
    setPlayingToFalse() {
      if (this.isPlaying && this.intervalId) {
        clearInterval(this.intervalId);
        this.isPlaying = false;
      }
    },
    changePlayInterval(time) {
      this.playIntervalTime = time;
    },

    changeSlideInterval(time) {
      this.slideInterval = time;
    },

    changeThumbnailPosition(position) {
      this.thumbnailPosition = position;
    },

    setPlayAndPause() {
      this.isPlaying = !this.isPlaying;
      if (this.isPlaying) {
        this.intervalId = setInterval(() => {
          this.goToNextSlide();
        }, this.playIntervalTime);
      } else {
        clearInterval(this.intervalId);
      }
    },
    nextSlide(arr, index) {
      let rightArr, leftArr;

      rightArr = arr.slice(index);
      leftArr = arr.slice(0, index);

      if (this.direction === "right") {
        rightArr.reduce((previousValue, nextValue, index) => {
          if (index === 0) return (nextValue.value = previousValue);
          return (nextValue.value = previousValue + 100);
        }, 0);
        leftArr.reverse().reduce((previousValue, nextValue) => {
          return (nextValue.value = previousValue - 100);
        }, 0);
      } else {
        rightArr.reduce((previousValue, nextValue) => {
          return (nextValue.value = previousValue + 100);
        }, 0);
        leftArr.reverse().reduce((previousValue, nextValue, index) => {
          if (index === 0) return (nextValue.value = previousValue);
          return (nextValue.value = previousValue - 100);
        }, 0);
      }

      return leftArr.reverse().concat(rightArr);
    },
    goToNextSlide() {
      this.currentIndex = this.currentIndex + 1;
      this.direction = "right";
      if (this.currentIndex === this.pictures.length) this.currentIndex = 0;
      this.nextSlide(this.pictures, this.currentIndex);
    },

    goToPrevSlide() {
      this.currentIndex = this.currentIndex - 1;
      this.direction = "left";
      if (this.currentIndex < 0) {
        this.currentIndex = 0;
        return;
      }
      const currentImageIndex = this.pictures.findIndex((elem) => {
        return elem.value === 0;
      });
      this.nextSlide(this.pictures, currentImageIndex);
    },
    onResize() {
      const currentWidth = this.$refs.slideRef.clientWidth;
      const currentheight = this.$refs.slideRef.clientHeight;
      const maxHeightForThumbnail = 770 - currentheight;
      const maxWidtForThumbnail = 1124 - currentWidth;
      this.thumbnailWidth = maxWidtForThumbnail;
      this.thumbnailHeight = currentheight;
      this.maxHeight = maxHeightForThumbnail;
    },
  },

  computed: {
    leftOrTop() {
      return (
        (this.checkboxes.includes("thumbnails") &&
          this.thumbnailPosition === "Top") ||
        this.thumbnailPosition === "Left"
      );
    },

    rightOrBottom() {
      return (
        (this.checkboxes.includes("thumbnails") &&
          this.thumbnailPosition === "Bottom") ||
        this.thumbnailPosition === "Right"
      );
    },

    classObject() {
      return {
        left:
          this.thumbnailPosition === "Left" ||
          this.thumbnailPosition === "Right",
      };
    },

    left() {
      return {
        horizontalWrapper:
          this.thumbnailPosition === "Left" ||
          this.thumbnailPosition === "Right",
      };
    },
  },

  created() {
    window.addEventListener("resize", this.onResize);
  },
  unmounted() {
    window.removeEventListener("resize", this.onResize);
  },

  mounted() {
    this.currentIndex = 0;
    this.resizeObserver = new ResizeObserver(this.onResize);
    this.resizeObserver.observe(this.$refs.slideRef);
    this.onResize();
  },
  beforeUnmount() {
    this.resizeObserver.unobserve(this.$refs.slideRef);
  },
};
</script>

<style lang="scss">
* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

.content {
  margin-top: 50px;
}

.image-gallery {
  margin: 0 auto;
  width: 65%;
  user-select: none;
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
  position: relative;

  @media (max-width: 1024px) {
    width: 100%;
  }
}

.image-gallery-content {
  position: relative;
  line-height: 0;
  top: 0;
}

.image-gallery-slide-wrapper {
  position: relative;
}

.image-gallery-icon {
  color: #fff;
  transition: all 0.3s ease-out;
  appearance: none;
  background-color: transparent;
  border: 0;
  cursor: pointer;
  outline: none;
  position: absolute;
  z-index: 4;
  filter: drop-shadow(0 2px 2px #1a1a1a);
}

.image-gallery-left-nav {
  padding: 50px 10px;
  top: 50%;
  left: 0;
  transform: translateY(-50%);
  font-size: 100px;
}
.image-gallery-right-nav {
  padding: 50px 10px;
  top: 50%;
  transform: translateY(-50%);
  right: 0;
  font-size: 100px;
}

.image-gallery-slides {
  line-height: 0;
  overflow: hidden;
  position: relative;
  white-space: nowrap;
  text-align: center;
}

.image-gallery-play-button {
  bottom: 0;
  padding: 20px;
}

.left {
  display: inline-block;
  vertical-align: top;
  width: 100px;
  margin: 0 5px;
  position: relative;
  writing-mode: vertical-lr;
}

.horizontalWrapper {
  display: inline-block;
  width: calc(100% - 110px);
}
</style>
