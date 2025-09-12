<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Search By Ingredient</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content v-if="pageState.loading">
      <section class="loading-center">
        <ion-spinner color="primary"></ion-spinner>
      </section>
    </ion-content>
    <ion-content :fullscreen="true" v-else>
      <ion-list>
        <ion-item v-for="ingredient in pageState.lstIngredients" :key="ingredient.strIngredient1"
          @click="() => router.push(`/drinks-by-ingredient/${ingredient.strIngredient1}`)">
          <ListCard 
            :img="ingredient.strIngredient1"
            :title="ingredient.strIngredient1"
          />
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
  import {
    IonPage, IonHeader, IonToolbar, IonTitle,
    IonContent, IonSpinner, IonAvatar,
    IonItem, IonLabel, IonList,
  } from '@ionic/vue';
  import { useRouter } from 'vue-router';
  import { reactive } from "vue";
  import axios from 'axios';
  import ListCard  from "@/components/ListCard.vue";
  import { Ingredient } from "@/types/index";

  const router = useRouter();
  const pageState = reactive({
    lstIngredients: [] as Ingredient[],
    loading: false
  });

  const fetchIngredients = async () => {
    pageState.loading = true;
    let res = await axios.get("https://www.thecocktaildb.com/api/json/v1/1/list.php?i=list");

    if (res.data) {
      pageState.lstIngredients = res.data?.drinks;
      pageState.lstIngredients.sort((a,b) => {
        return a.strIngredient1.localeCompare(b.strIngredient1);
      });
    }

    pageState.loading = false;
  }

  fetchIngredients();

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
