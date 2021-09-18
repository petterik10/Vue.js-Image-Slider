<template>
  <section class="header">
    <h1 class="gallery-header">Vue Image Slider</h1>
    <h3 class="gallery-header-3">
      Responsive carousel app with thumbnail support
    </h3>
  </section>
</template>

<script>
export default {
  name: "Header",
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.header {
  background: #178ccb;
  padding: 20px 0;
  color: #fff;
  width: 65%;
  margin: auto;

  @media (max-width: 1024px) {
    width: 100%;
  }
}

.gallery-header {
  font-size: 38px;
  text-align: center;
  text-transform: uppercase;
  margin: 0;
  padding: 20px 0 10px;
  font-weight: 600;
}

.gallery-header-3 {
  text-align: center;
  font-weight: 300;
  margin: 0;
}
</style>
