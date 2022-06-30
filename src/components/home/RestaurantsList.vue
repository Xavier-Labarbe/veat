<script lang="ts">
import { IonText } from "@ionic/vue";
import RestaurantCard from "./RestaurantCard.vue";

export default {
  name: "restaurants-list",
  props: ["restaurants"],
  components: {
    IonText,
    RestaurantCard,
  },
};
</script>

<template>
  <ion-text color="dark"
    ><b style="padding-left: 10px">Restaurants</b></ion-text
  >
  <restaurant-card
    v-for="(restaurant, key) in restaurants"
    :key="key"
    :router-link="'/restaurant/'+(restaurant as any).name"
    :restaurant="restaurant"
  ></restaurant-card>
</template>
