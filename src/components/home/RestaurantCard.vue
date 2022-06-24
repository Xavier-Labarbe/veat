<script lang="ts">
import {
  IonText,
  IonIcon,
  IonRow,
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
    IonCard,
    IonCardHeader,
    IonCardContent,
    IonCardTitle,
  },
};
</script>
<template>
  <ion-card :router-link="'/restaurant/'+(restaurant as any).name">
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
            <ion-text color="secondary">{{(restaurant as any).rates}}</ion-text>
          </span>
        </ion-row>
        <ion-row>
          <span>
            <ion-icon name="location-outline"></ion-icon>
            {{ (restaurant as any).city }}, {{ ((restaurant as any).address)}}
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
