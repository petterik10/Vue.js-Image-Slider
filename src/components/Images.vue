<template>
  <div :key="picture.color" v-for="picture in pictures.colors">
    <div class="image fade" v-if="picture.id === currentPicture.id">
      <div>
        <img
          :alt="picture.description"
          :src="require(`@/assets/${picture.src}`)"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Images",
  props: {
    pictures: Array,
    currentPicture: Array,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.image {
  height: 400px;
  width: 400px;
  overflow: hidden;
  margin: auto;
}

/* Fading animation */
.fade {
  -webkit-animation-name: fade;
  -webkit-animation-duration: 1.5s;
  animation-name: fade;
  animation-duration: 1.5s;
}

@-webkit-keyframes fade {
  from {
    opacity: 0.4;
  }
  to {
    opacity: 1;
  }
}

@keyframes fade {
  from {
    opacity: 0.4;
  }
  to {
    opacity: 1;
  }
}
</style>
