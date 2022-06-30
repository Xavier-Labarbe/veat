<template>
  <ion-page ref="page">
    <ion-header>
      <ion-toolbar color="light">
        <ion-buttons slot="start">
          <ion-button
            fill="solid"
            shape="round"
            color="light"
          >
            <ion-back-button default-href="/">
              <ion-icon
                  slot="icon-only"
                  name="arrow-back"
                  color="primary"
              ></ion-icon>
            </ion-back-button>
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
    <ion-content class="content" :ionFullscreeen="true">
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

      <ion-list v-for="(category, key) in data.food" :key="key">
        <ion-list-header>
          <ion-label>{{ category?.category }}</ion-label>
        </ion-list-header>
        <ion-row
          v-for="(meal, mealKey) in category.meals"
          :key="mealKey"
          class="ion-padding meal-row"
          expand="block" @click="setOpenFood(true, key, mealKey)"
        >
          <food-modal :is-open="isOpenFood" @openState="this.isOpenFood = false"  @addProduct="addToOrder" :meal="data.food[categoryKey].meals[modalKey]"></food-modal>
          <ion-col size="8" class="border-bottom">
            <ion-label>
              {{ meal?.name }}
              <p>{{ meal?.info }}</p>
            </ion-label>
            <ion-text color="dark"
              ><b>{{ meal?.price }}</b></ion-text
            >
          </ion-col>
          <ion-col size="4" class="border-bottom">
            <img :src="meal?.img" />
          </ion-col>
        </ion-row>
      </ion-list>

      <order-modal  :is-open="isOpenOrder" @openState="this.isOpenOrder = false" :order="order" :total="getTotal(order)" :restaurant-id="data.id" @updateOrder="updateOrder"></order-modal>

    </ion-content>
    <ion-footer class="footer" collapse="fade">
      <ion-toolbar>
        <ion-button class="bar bar-footer" @click="setOpenOrder(true)">Voire la commande</ion-button>
      </ion-toolbar>
    </ion-footer>
  </ion-page>
</template>

<script lang="ts">
import { IonContent, IonButtons, IonPage, IonList, IonIcon, IonFooter, IonBackButton, IonButton, IonTitle, IonToolbar, IonHeader, IonText, IonCol, IonRow, IonLabel, IonItem, IonListHeader, IonModal, actionSheetController, } from "@ionic/vue";
import { defineComponent } from "vue";
import { useRoute, useRouter } from "vue-router";
import axios from "axios";
import FoodModal from "@/components/Restaurant/FoodModal.vue";
import OrderModal from "@/components/Restaurant/OrderModal.vue";

export default defineComponent({
  name: "RestaurantPage",
  components: {
    IonContent,
    IonButtons,
    IonPage,
    IonList,
    IonBackButton,
    IonIcon,
    IonButton,
    IonTitle,
    IonToolbar,
    IonHeader,
    IonText,
    IonModal,
    IonFooter,
    IonCol, IonRow, IonLabel, IonItem,
    IonListHeader,
    actionSheetController,
    FoodModal, OrderModal
  },
  data() {
    return {
      data: {
        id: String,
        name: String,
      },
      isOpenOrder: false,
      isOpenFood: false,
      modalKey: 0,
      categoryKey: 0,
      order: [null],
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
    setOpenFood(isOpen: boolean, categoryKey: number, modalKey: number) {
      this.categoryKey = categoryKey;
      this.modalKey = modalKey;
      this.isOpenFood = isOpen;
    },
    setOpenOrder(isOpen: boolean){
      this.isOpenOrder = isOpen;
    },
    addToOrder(product: any){
      var exist = false;
      var isNull = -1;
      this.order.forEach((p, index)=>{
        if ((p as any)?.meal?.id == product.meal.id) {
          (p as any).quantity = product.quantity
          exist = true;
          if((p as any).quantity == 0){
            isNull = index;
          }
        }
      });
      if(!exist){
        this.order.push(product)
      }
      if(isNull != -1){
        this.order.splice(isNull)
      }
    },
    updateOrder(order: any){
      console.log(order)
      this.order = order;
      this.order.forEach((p, index)=>{
        if((p as any)?.quantity == 0){
          this.order.splice(index)
        }
      });
    },
    getTotal(order: any){
      var total = 0;
      order.forEach((o: any, index: number)=>{
        if(o){
          total += Number((o?.meal?.price*o?.quantity).toFixed(2))
        }
      });
      return total
    }
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
.content {
  min-height: calc(100vh - 70px);
}

.footer {
  text-align: center;
  height: 50px;
}

ion-icon {
  font-size: 25px;
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
