<template>
  <ion-modal>
    <ion-header>
      <ion-toolbar>
        <ion-buttons slot="end">
          <ion-button @click="setOpen(false)">Close</ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>
    <ion-content class="content">
      <ion-img :src="meal?.img"></ion-img>
      <ion-row>
        <ion-col class="light-bg ion-padding-start">
          <ion-text color="dark">
            <h4>
              <strong>
                {{ meal?.name }}
              </strong>
            </h4>
          </ion-text>
        </ion-col>
      </ion-row>
      <ion-row>
        <ion-col class="light-bg ion-padding-start">
          <ion-text color="dark">
            <h4>
              <strong>
                {{ meal?.price }}€
              </strong>
            </h4>
          </ion-text>
        </ion-col>
      </ion-row>
      <ion-row>
        <ion-col class="light-bg ion-padding-start ion-text-center">
          <ion-button class="ion-padding-end" v-on:click="addQuantity">+</ion-button>
          <ion-text color="dark">
            {{quantity}}
          </ion-text>
          <ion-button class="ion-padding-start" v-on:click="removeQuantity">-</ion-button>
        </ion-col>
      </ion-row>
      <ion-footer class="footer" collapse="fade">
        <ion-toolbar>
          <ion-button class="bar bar-footer" @click="setOpen(false)">Ajouter à la commande</ion-button>
        </ion-toolbar>
      </ion-footer>
    </ion-content>
  </ion-modal>
</template>

<script lang="ts">
import { IonButtons, IonFooter, IonButton, IonTitle, IonHeader, IonModal, IonContent, IonToolbar, IonImg, IonRow, IonCol, IonText} from "@ionic/vue";
import {defineComponent} from "vue";
export default defineComponent({
  name: "food-modal",
  components: {IonButtons, IonFooter, IonButton, IonTitle, IonHeader, IonModal, IonContent, IonToolbar, IonImg, IonRow, IonCol, IonText},
  data(){
    return{
      quantity: 0,
    }
  },
  props:{
    meal: {}
  },
  methods:{
    setOpen(state: boolean) {
      this.$emit("openState", state)
    },
    addQuantity(){
      this.quantity += 1;
    },
    removeQuantity(){
      if(this.quantity != 0){
        this.quantity -= 1;
      }
    }
  }
})
</script>

<style scoped>

</style>