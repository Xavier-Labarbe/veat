<template>
  <ion-modal>
    <ion-header>
      <ion-toolbar>
        <ion-buttons slot="end">
          <ion-button @click="setOpen(false, order)">Close</ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>
    <ion-content class="content">
      <ion-row>
        <ion-col class="light-bg ion-padding-start">
          <ion-text color="dark">
            <h4>
              <strong>
                Résumé de la commande
              </strong>
            </h4>
          </ion-text>
        </ion-col>
      </ion-row>
      <ion-row class="light-bg ion-padding-start" v-for="(product, key) in order" :key="key">
        <ion-col size="8" class="border-bottom" v-if="product!=null">
          <ion-label>
            {{ product?.meal?.name }}
            <p>{{ product?.meal?.price }}</p>
          </ion-label>
          <ion-button class="ion-padding-end" v-on:click="product.quantity = removeQuantity(product.quantity); total = getTotal(order)">-</ion-button>
          <ion-text color="dark">
            {{product.quantity}}
          </ion-text>
          <ion-button class="ion-padding-start" v-on:click="product.quantity = addQuantity(product.quantity); total = getTotal(order)">+</ion-button>
        </ion-col>
        <ion-col size="4" class="border-bottom">
          <img :src="product?.meal?.img" />
        </ion-col>
      </ion-row>
      <ion-row>
        <ion-col class="light-bg ion-padding-start" size="9" >
          <ion-text color="dark">
            <h4>
              <strong>
                Total :
              </strong>
            </h4>
          </ion-text>
        </ion-col>
        <ion-col class="light-bg ion-padding-start" size="3">
          <ion-text color="dark">
            <h4>
              <strong>
                {{total}}€
              </strong>
            </h4>
          </ion-text>
        </ion-col>
      </ion-row>
      <ion-footer class="footer" collapse="fade">
        <ion-toolbar>
          <ion-button class="bar bar-footer" @click="validateOrder(order, total)">Valider la commande</ion-button>
        </ion-toolbar>
      </ion-footer>
    </ion-content>
  </ion-modal>
</template>

<script lang="ts">
import { IonButtons, IonFooter, IonButton, IonTitle, IonHeader, IonModal, IonContent, IonToolbar, IonImg, IonRow, IonCol, IonText,IonLabel} from "@ionic/vue";
import {defineComponent} from "vue";
import axios from "axios";
export default defineComponent({
  name: "order-modal",
  components: {IonButtons, IonFooter, IonButton, IonTitle, IonHeader, IonModal, IonContent, IonToolbar, IonImg, IonRow, IonCol, IonText, IonLabel},
  data(){
    return{
    }
  },
  props:{
    order: Array,
    total: Number,
  },
  methods:{
    setOpen(state: boolean, order?: any) {
      if(order){
        this.$emit("updateOrder", order)
      }
      this.$emit("openState", state)
    },
    addQuantity(quantity: number){
      return quantity += 1;
    },
    removeQuantity(quantity: number){
      if(quantity != 0){
        return quantity -= 1;
      }
      return 0;
    },
    getTotal(order: any){
      var total = 0;
      order.forEach((o: any, index: number)=>{
        if(o){
          total += Number((o?.meal?.price*o?.quantity).toFixed(2))
        }
      });
      return total
    },
    async validateOrder(order: any, total: number){
      ///NEED PAYMENT METHOD

      ///
      await axios.post(
          "http://localhost:3000/addOrder", {meals: order, total: total}
      )
      this.setOpen(false);
      this.$router.push('/')
    }
  },
})
</script>

<style scoped>

</style>