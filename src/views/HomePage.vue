<template>
  <ion-content scrollEvents="true" @ionScroll="onScroll($event)">
    <ion-refresher slot="fixed" @ionRefresh="doRefresh($event)">
      <ion-refresher-content></ion-refresher-content>
    </ion-refresher>

    <ion-row class="info-row">
      <ion-col size="2" class="ion-padding-start">
        <img src="../assets/delivery.png" />
      </ion-col>
      <ion-col size="8">
        <ion-text color="dark">
          <span>Travail, Maintenant</span><br />
          <b>Bordeaux</b>
          <ion-icon name="chevron-down-outline" color="primary"></ion-icon>
        </ion-text>
      </ion-col>
      <ion-col size="2">
        <ion-button fill="clear">
          <ion-icon name="person-outline"></ion-icon>
        </ion-button>
      </ion-col>
    </ion-row>

    <div class="sticky-row">
      <ion-row>
        <ion-col>
          <ion-text
            class="ion-padding-start"
            color="dark"
            v-bind:class="{
              'location-visible': showLocationDetail,
              'location-hidden': !showLocationDetail,
            }"
          >
            <b>Bordeaux</b>
          </ion-text>
        </ion-col>
      </ion-row>
      <ion-row>
        <ion-col size="10">
          <ion-searchbar
            placeholder="Restaurants, commerces, plats"
          ></ion-searchbar>
        </ion-col>
        <ion-col size="2">
          <ion-button fill="clear">
            <ion-icon name="options-outline"></ion-icon>
          </ion-button>
        </ion-col>
      </ion-row>
    </div>

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

    <ion-text color="dark"><b style="padding-left: 10px">A là une</b></ion-text>

    <swiper :slidesPerView="1.2" :spaceBetween="10">
      <swiper-slide
        v-for="(f, index) in featured"
        :key="index"
        class="featured-slide"
      >
        <ion-card :router-link="'/restaurant/'+(f as any).name">
          <img :src="(f as any).img" alt="test" />
          <div class="info">
            <ion-card-header>
              <ion-card-title color="dark"
                ><b>{{(f as any).name}}</b>
              </ion-card-title>
            </ion-card-header>

            <ion-card-content>
              <ion-row>
                <span>
                  <ion-icon name="star-outline" color="secondary"></ion-icon>
                  <ion-text color="secondary">{{(f as any).rating}}</ion-text>
                </span>
              </ion-row>
              <ion-row>
                <span>
                  <ion-icon name="location-outline"></ion-icon>
                  {{ (f as any).distance }}
                </span>
              </ion-row>
            </ion-card-content>
          </div>
        </ion-card>
      </swiper-slide>
    </swiper>

    <ion-text color="dark"
      ><b style="padding-left: 10px">Restaurants</b></ion-text
    >
    <ion-card
      v-for="(restaurant, key) in featured"
      :key="key"
      :router-link="'/restaurant/'+(restaurant as any).name"
    >
      <img :src="(restaurant as any).img" alt="test" />
      <div class="info">
        <ion-card-header>
          <ion-card-title color="dark"
            ><b>{{(restaurant as any).name}}</b>
          </ion-card-title>
        </ion-card-header>

        <ion-card-content>
          <ion-row>
            <span>
              <ion-icon name="star-outline" color="secondary"></ion-icon>
              <ion-text
                color="secondary"
                >{{(restaurant as any).rating}}</ion-text
              >
            </span>
          </ion-row>
          <ion-row>
            <span>
              <ion-icon name="location-outline"></ion-icon>
              {{ (restaurant as any).distance }}
            </span>
          </ion-row>
        </ion-card-content>
      </div>
    </ion-card>
  </ion-content>
</template>

<script lang="ts">
import {
  IonContent,
  IonText,
  IonIcon,
  IonRefresher,
  IonRefresherContent,
  IonRow,
  IonCol,
  IonSearchbar,
  IonButton,
  IonCard,
  IonCardTitle,
  IonCardHeader,
  IonCardContent,
} from "@ionic/vue";
import { defineComponent } from "vue";
import { useRouter } from "vue-router";
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
    IonRefresher,
    IonRefresherContent,
    IonRow,
    IonCol,
    IonSearchbar,
    IonButton,
    IonCard,
    IonCardHeader,
    IonCardContent,
    IonCardTitle,
    Swiper,
    SwiperSlide,
  },
  data() {
    return {
      categories: [],
      featured: [],
      showLocationDetail: false,
    };
  },
  methods: {
    getData: function () {
      axios
        .get("https://devdactic.fra1.digitaloceanspaces.com/foodui/home.json")
        .then((response) => {
          this.categories = response.data.categories;
          this.featured = response.data.featured;
        });
    },
    doRefresh: function (event: any) {
      setTimeout(() => {
        event.target.complete();
      }, 2000);
    },
    onScroll: function ($ev: any) {
      const offset = $ev.detail.scrollTop;
      this.showLocationDetail = offset > 40;
    },
  },
  setup() {
    const router = useRouter();
    return { router };
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

ion-content {
  --padding-top: 40px;
}

.sticky-row {
  position: sticky;
  top: calc(env(safe-area-inset-top) - 40px);
  z-index: 2;
  background: #fff;
  box-shadow: 0px 9px 11px -15px rgba(0, 0, 0, 0.75);
}

.info-row {
  background: #fff;
  position: sticky;
  top: calc(env(safe-area-inset-top) - 40px);
  z-index: 2;
}

ion-refresher {
  padding-top: calc(env(safe-area-inset-top) + 50px);
}

ion-searchbar {
  --icon-color: var(--ion-color-medium);
}

.location-visible {
  opacity: 1;
  transition: 0.5s;
}

.location-hidden {
  opacity: 0;
  transition: 0.5s;
}
</style>
