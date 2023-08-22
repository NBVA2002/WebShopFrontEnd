<template>
  <a-carousel autoplay>
    <div class="slider slider1"></div>
    <div class="slider slider2"></div>
    <div class="slider slider3"></div>
    <div class="slider slider4"></div>
  </a-carousel>
  
</template>

<style scoped>
/* For demo */
:deep(.slick-slide) {
  text-align: center;
  height: 700px;
  line-height: 160px;
  background: #364d79;
  overflow: hidden;
}
:deep(.slick-slide h3) {
  color: #fff;
}

.slider {
  width: 100%;
  height: 700px;
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
}

.slider1 {
  background-image: url("../assets/images/banner/banner.png");
}

.slider2 {
  background-image: url("../assets/images/banner/lb_2_98_44_anhslide.jpg");
}

.slider3 {
  background-image: url("../assets/images/banner/b19.jpg");
}

.slider4 {
  background-image: url("../assets/images/banner/lb_3_97_45_anhslide.jpg");
}
</style>