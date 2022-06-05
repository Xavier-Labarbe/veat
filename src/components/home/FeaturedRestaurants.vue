<script lang="ts">
import { IonText } from "@ionic/vue";
import "@ionic/vue/css/ionic-swiper.css";
import { Swiper, SwiperSlide } from "swiper/vue";
import RestaurantCard from "./RestaurantCard.vue";
export default {
  name: "featured-restaurants",
  props: ["featured"],
  components: {
    IonText,
    Swiper,
    SwiperSlide,
    RestaurantCard,
  },
};
</script>
<template>
  <ion-text color="dark"><b style="padding-left: 10px">A là une</b></ion-text>

  <swiper :slidesPerView="1.2" :spaceBetween="10">
    <swiper-slide
      v-for="(f, index) in featured"
      :key="index"
      class="featured-slide"
    >
      <restaurant-card :restaurant="f"></restaurant-card>
    </swiper-slide>
  </swiper>
</template>
