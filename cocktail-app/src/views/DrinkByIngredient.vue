<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-back-button></ion-back-button>
        </ion-buttons>
        <ion-title>{{ ingredient }} Drinks</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content v-if="pageState.loading">
      <section class="loading-center">
        <ion-spinner color="primary"></ion-spinner>
      </section>
    </ion-content>
    <ion-content :fullscreen="true" v-else>
      <ion-list>
        <ion-item v-for="drink in pageState.lstDrinks" :key="drink.idDrink"
          @click="() => {
            router.push(`/drinks-info/${drink.idDrink}`)
          }"
          >
          <DrinkListCard :img="drink.strDrinkThumb" :title="drink.strDrink" />
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
  import {
    IonPage, IonHeader, IonToolbar, IonTitle,
    IonContent, IonSpinner, IonItem, IonList,
    IonBackButton,IonButtons,
  } from '@ionic/vue';
  import { useRouter, useRoute } from 'vue-router';
  import { reactive } from "vue";
  import axios from 'axios';
  import DrinkListCard from "@/components/DrinkListCard.vue";
  import { Drink } from "@/types/index";

  const route = useRoute();
  const router = useRouter();
  const ingredient = route.params.ingredient;


  const pageState = reactive({
    lstDrinks: [] as Drink[],
    loading: false
  });

  const fetchIngredientsDrinks = async () => {
    pageState.loading = true;
    let res = await axios.get(`https://www.thecocktaildb.com/api/json/v1/1/filter.php?i=${ingredient}`);
    if (res) {
      pageState.lstDrinks = res.data?.drinks;
    }

    pageState.loading = false;
  }

  fetchIngredientsDrinks();

</script>
<style lang="css">
  .loading-center {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 90dvh;
  }

  ion-spinner {
    transform: scale(1.5);
  }
</style>
