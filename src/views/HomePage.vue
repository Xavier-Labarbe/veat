<template>
  <ion-content>
    <swiper :slidesPerView="3.5" :spaceBetween="10" :slidesOffsetBefore="11">
      <swiper-slide v-for="(cat, key) in categories" :key="key">
        <img :src="(cat as any).img" alt="test" />
      </swiper-slide>
    </swiper>

    <swiper
      :slidesPerView="1.05"
      :spaceBetween="10"
      :centeredSlides="true"
      :loop="true"
    >
      <swiper-slide v-for="(h, index) in highlights" :key="index">
        <img :src="(h as any).img" alt="test" />
      </swiper-slide>
    </swiper>

    <ion-text color="dark"><b style="padding-left: 10px">Featured</b></ion-text>

    <swiper :slidesPerView="1" :spaceBetween="10">
      <swiper-slide
        v-for="(f, index) in featured"
        :key="index"
        class="featured-slide"
      >
        <img :src="(f as any).img" alt="test" />
        <div class="info">
          <ion-text color="dark"
            ><b>{{(f as any).name}}</b>
          </ion-text>
          <span>
            <ion-icon name="star-outline" color="secondary"></ion-icon>
            <ion-text color="secondary">{{(f as any).rating}}</ion-text>
          </span>
          <span>
            <ion-icon name="location-outline"></ion-icon>
            {{ (f as any).distance }}
          </span>
        </div>
      </swiper-slide>
    </swiper>
  </ion-content>
</template>

<script lang="ts">
import { IonContent, IonText, IonIcon } from "@ionic/vue";
import { defineComponent } from "vue";
import "swiper/scss";
import "@ionic/vue/css/ionic-swiper.css";
import { Swiper, SwiperSlide } from "swiper/vue";
import axios from "axios";

export interface ICat {
  title: string;
  img: string;
}

export default defineComponent({
  name: "HomePage",
  components: {
    IonContent,
    IonText,
    IonIcon,
    Swiper,
    SwiperSlide,
  },
  data() {
    return {
      categories: [],
      highlights: [],
      featured: [],

      featuredSlideOpts: {
        slidesPerView: 1.2,
        spaceBetween: 10,
        freeMode: true,
      },
    };
  },
  methods: {
    getData: function () {
      axios
        .get("https://devdactic.fra1.digitaloceanspaces.com/foodui/home.json")
        .then((response) => {
          this.categories = response.data.categories;
          this.highlights = response.data.highlights;
          this.featured = response.data.featured;
        });
    },
  },
  mounted() {
    this.getData();
  },
});
</script>

<style scoped>
ion-slides {
  padding-left: 15px;
  padding-right: 15px;
  margin-top: 15px;
  margin-bottom: 15px;
}

.featured-slide {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.info {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}
</style>
