<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Random Drink</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content v-if="state.loading">
      <section class="loading-center">
        <ion-spinner color="primary"></ion-spinner>
      </section>
    </ion-content>
    <ion-content :fullscreen="true" v-else>
      <ion-refresher slot="fixed" @ionRefresh="doRefresh">
        <ion-refresher-content></ion-refresher-content>
      </ion-refresher>
      <DrinkCard :drink=state.randomCocktail />
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

  const state = reactive({
    randomCocktail: {} as Drink,
    loading: false
  });

  const fetchRandomCocktail = async (displayLoader: boolean) => {
    if (displayLoader) {
      state.loading = true;
    }

    const res = await axios.get("https://www.thecocktaildb.com/api/json/v1/1/random.php");

    if (res.data) {
      state.randomCocktail = res.data?.drinks[0];
    }

    state.loading = false;
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
