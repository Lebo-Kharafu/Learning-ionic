<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>{{ pageState.drink.strDrink }}</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content v-if="pageState.loading">
      <section class="loading-center">
        <ion-spinner color="primary"></ion-spinner>
      </section>
    </ion-content>
    <ion-content :fullscreen="true" v-else>
      <ion-refresher slot="fixed" @ionRefresh="doRefresh">
        <ion-refresher-content></ion-refresher-content>
      </ion-refresher>
      <DrinkCard :drink=pageState.drink />
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
  import {
    IonPage, IonHeader, IonToolbar, IonTitle,
    IonContent, IonSpinner, IonRefresher, IonRefresherContent,
  } from '@ionic/vue';
  import { reactive } from 'vue';
  import axios from 'axios';
  import DrinkCard from "@/components/DrinkCard.vue";
  import { Drink } from "@/types/index";
  import { useRoute } from 'vue-router';

  const route = useRoute();
  const idDrink = route.params.idDrink;

  const pageState = reactive({
    drink: {} as Drink,
    loading: false
  });

  const fetchRandomCocktail = async (displayLoader: boolean) => {
    if (displayLoader) {
      pageState.loading = true;
    }
    //@ts-ignore
    const res = await axios.get(`https://www.thecocktaildb.com/api/json/v1/1/lookup.php?i=${idDrink}`);
    if (res.data) {
      pageState.drink = res.data?.drinks[0];
    }

    pageState.loading = false;
  }


  const doRefresh = async (event: CustomEvent) => {
    await fetchRandomCocktail(false);
    //@ts-ignore
    event.target?.complete();
  }

  fetchRandomCocktail(true);

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
