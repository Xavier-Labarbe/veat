<template>
  <ion-page>
    <ion-content scrollEvents="true" @ionScroll="onScroll($event)">
      <ion-refresher slot="fixed" @ionRefresh="doRefresh($event)">
        <ion-refresher-content></ion-refresher-content>
      </ion-refresher>

      <home-header />

      <home-categories :categories="categories" />

      <featured-restaurants :featured="featured" />

      <restaurants-list :restaurants="restaurants" />
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonContent, IonRefresher, IonRefresherContent, IonPage } from "@ionic/vue";
import { defineComponent } from "vue";
import { useRouter } from "vue-router";
import FeaturedRestaurants from "../components/home/FeaturedRestaurants.vue";
import RestaurantsList from "@/components/home/RestaurantsList.vue";
import HomeHeader from "@/components/home/HomeHeader.vue";
import HomeCategories from "@/components/home/HomeCategories.vue";
import axios from "axios";

export default defineComponent({
  name: "HomePage",
  components: {
    IonContent,
    IonRefresher,
    IonRefresherContent,
    IonPage,
    FeaturedRestaurants,
    RestaurantsList,
    HomeHeader,
    HomeCategories,
  },
  data() {
    return {
      categories: [],
      featured: [],
      restaurants: [],
      showLocationDetail: false,
    };
  },
  methods: {
    getData: async function () {
      await axios
        .get("https://devdactic.fra1.digitaloceanspaces.com/foodui/home.json")
        .then((response) => {
          this.categories = response.data.categories;
        });
    },
    data: function () {
      axios
        .get("http://localhost:3000/getHome")
        .then((response) => {
          this.featured = response.data.featured;
          this.restaurants = response.data.restaurants;
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
    this.data();
    this.getData();
  },
});
</script>

<style scoped>
ion-refresher {
  padding-top: calc(env(safe-area-inset-top) + 50px);
}
</style>
