<script lang="ts">
import { IonText, IonIcon, IonRow, IonButton, IonSearchbar } from "@ionic/vue";

export default {
  name: "home-header",
  components: { IonText, IonIcon, IonRow, IonButton, IonSearchbar },
};
</script>

<template>
  <div class="sticky-row">
    <ion-row class="info-row">
      <ion-col size="2" class="ion-padding-start">
        <img src="../../assets/icon.png" />
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
</template>

<style>
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
