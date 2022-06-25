<template>
  <ion-page>
    <ion-header>
      <ion-toolbar color="light">
        <ion-buttons slot="start">
          <ion-button
            fill="solid"
            shape="round"
            color="light"
            :router-link="'/'"
          >
            <ion-icon
              slot="icon-only"
              name="arrow-back"
              color="primary"
            ></ion-icon>
          </ion-button>
        </ion-buttons>
        <ion-title>{{ data?.name }}</ion-title>
        <ion-buttons slot="end">
          <ion-button fill="solid" shape="round" color="light">
            <ion-icon
              slot="icon-only"
              name="share-outline"
              color="primary"
            ></ion-icon>
          </ion-button>
          <ion-button fill="solid" shape="round" color="light">
            <ion-icon
              slot="icon-only"
              name="search-outline"
              color="primary"
            ></ion-icon>
          </ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>
    <ion-content :ionFullscreeen="true">
      <div id="img" class="header-image">
        <img :src="data?.img" />
      </div>
      <ion-header collapse="condense" class="ion-no-border">
        <ion-toolbar color="light">
          <ion-title size="large">{{ data?.name }}</ion-title>
        </ion-toolbar>
      </ion-header>
      <div v-if="data">
        <ion-row>
          <ion-col class="light-bg ion-padding-start">
            <ion-text color="dark">
              <h4>
                <strong>
                  {{ data?.name }}
                </strong>
              </h4>
            </ion-text>
          </ion-col>
          <ion-col size="12" class="light-bg ion-padding-start">
            <ion-text color="medium">
              <ion-icon name="star" color="primary"></ion-icon>
              <ion-text color="primary"> {{ data.rates }}</ion-text>
              <span v-for="tag in data.tags" :key="tag.name">
                · {{ tag?.name }}
              </span>
            </ion-text>
          </ion-col>
          <ion-col size="12" class="ion-no-padding">
            <ion-item lines="full">
              <ion-label class="ion-text-wrap">
                {{ data?.about }}
              </ion-label>
            </ion-item>
          </ion-col>
          <ion-col size="12" class="light-bg">
            <ion-row class="ion-align-items-center">
              <ion-col size="1">
                <ion-icon color="medium" name="location-outline"></ion-icon>
              </ion-col>
              <ion-col size="10">
                <ion-label
                  >Informations Restaurant
                  <p>Map, allergene ad hygiene rating</p>
                </ion-label>
              </ion-col>
              <ion-col size="1">
                <ion-icon color="primary" name="chevron-forward"></ion-icon>
              </ion-col>
            </ion-row>
          </ion-col>
        </ion-row>
      </div>

      <ion-list v-for="(entry, key) in data.food" :key="key">
        <ion-list-header>
          <ion-label>{{ entry?.category }}</ion-label>
        </ion-list-header>
        <ion-row
          v-for="(meal, key) in entry.meals"
          :key="key"
          class="ion-padding meal-row"
        >
          <ion-col size="8" class="border-bottom">
            <ion-label>
              {{ meal?.name }}
              <p>{{ meal.info }}</p>
            </ion-label>
            <ion-text color="dark"
              ><b>{{ meal.price }}</b></ion-text
            >
          </ion-col>
          <ion-col size="4" class="border-bottom">
            <img :src="meal?.img" />
          </ion-col>
        </ion-row>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonContent } from "@ionic/vue";
import { defineComponent } from "vue";
import { useRoute, useRouter } from "vue-router";
import axios from "axios";

export default defineComponent({
  name: "RestaurantPage",
  components: {
    IonContent,
  },
  data() {
    return {
      data: {
        name: String,
      },
      showLocationDetail: false,
      active_category: 0,
      listElements: [],
      categorySlidesVisible: false,
    };
  },
  methods: {
    doRefresh: function (event: any) {
      setTimeout(() => {
        event.target.complete();
      }, 2000);
    },
    getData: async function () {
      const headers = {
        "Content-Type": "application/json",
        "Access-Control-Allow-Origin": "*",
      };
      await axios
        .get("http://localhost:3000/getRestaurant/" + this.name, { headers })
        .then((response) => {
          this.data = response.data;
        });
    },
  },
  setup() {
    const route = useRoute();
    const { name } = route.params;
    return { name };
  },
  mounted() {
    this.getData();
  },
});
</script>

<style>
ion-toolbar {
  ion-icon {
    font-size: 25px;
  }
}

ion-content {
  position: absolute;
  --background: #f1f1f1;
  --padding-bottom: 50px;
}

.light-bg {
  background: #ffffff;
  z-index: 10;
}

.header-image {
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
  height: 23vh;
  will-change: transform;
}

ion-list-header {
  --background: #f1f1f1;
}

ion-list {
  --ion-background-color: #fff;
}

.meal-row {
  padding-bottom: 0px;
}

.meal-image {
  width: 100%;
  height: 100%;
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
  border-radius: 5px;
}

.border-bottom {
  border-bottom: 1px solid var(--ion-color-step-150, rgba(0, 0, 0, 0.07));
}

.active-category {
  --background: var(--ion-color-primary);
  --color: #fff;
  --border-radius: 30px;
  font-weight: 600;
}
</style>
