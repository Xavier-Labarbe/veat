<script lang="ts">
import "swiper/scss";
import "@ionic/vue/css/ionic-swiper.css";
import { Swiper, SwiperSlide } from "swiper/vue";

export default {
  name: "home-categories",
  props: ["categories"],
  components: {
    Swiper,
    SwiperSlide,
  },
};
</script>
<template>
  <swiper
    :slidesPerView="3.5"
    :spaceBetween="10"
    :slidesOffsetBefore="11"
    style="margin-bottom: 10px"
  >
    <swiper-slide v-for="(cat, key) in categories" :key="key">
      <img :src="(cat as any).img" alt="test" />
    </swiper-slide>
  </swiper>
</template>
