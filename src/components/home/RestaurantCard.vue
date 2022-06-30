<script lang="ts">
import {
  IonText,
  IonIcon,
  IonRow,
  IonCol,
  IonCard,
  IonCardTitle,
  IonCardHeader,
  IonCardContent,
} from "@ionic/vue";

export default {
  name: "restaurant-card",
  props: ["restaurant"],
  components: {
    IonText,
    IonIcon,
    IonRow,
    IonCol,
    IonCard,
    IonCardHeader,
    IonCardContent,
    IonCardTitle,
  },
};
</script>
<template>
  <ion-card :router-link="'/restaurant/'+restaurant.name">
    <img :src="restaurant.img" alt="test" />
    <div class="info">
      <ion-card-header>
        <ion-card-title color="dark"
          ><b>{{restaurant.name}}</b>
        </ion-card-title>
      </ion-card-header>

      <ion-card-content>
        <ion-row>
          <span>
            <ion-icon name="star-outline" color="secondary"></ion-icon>
            <ion-text color="secondary">{{restaurant.rates}}</ion-text>
          </span>
        </ion-row>
        <ion-row>
          <span>
            <ion-icon name="location-outline"></ion-icon>
            {{ restaurant.city }}, {{ restaurant.address}}
          </span>
        </ion-row>
      </ion-card-content>
    </div>
  </ion-card>
</template>
<style>
.info {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}
</style>
