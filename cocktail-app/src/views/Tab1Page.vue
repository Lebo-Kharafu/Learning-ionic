<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Tab 1</ion-title>
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
      <ion-card>
        <!-- @vue-ignore -->
        <img :src="state.randomCocktail.strDrinkThumb"></img>
        <ion-card-header>
          <ion-card-subtitle>
            <!-- @vue-ignore -->
            {{ state.randomCocktail.strCategory }} | Served In
            {{ state.randomCocktail.strGlass }}
          </ion-card-subtitle>
          <ion-card-title>
            <!-- @vue-ignore -->
            {{ state.randomCocktail.strDrink }}
          </ion-card-title>
        </ion-card-header>
        <ion-card-content>
          <p>
            <!-- @vue-ignore -->
            {{ state.randomCocktail.strInstructions }}
          </p>
          <ion-list-header>
            Ingredients
          </ion-list-header>
          <ion-list>
            <template v-for="indx in 15" :key="indx">
              <!-- @vue-ignore -->
              <ion-item v-if="state.randomCocktail[`strIngredient${indx}`]">
                <ion-label>
                  <!-- @vue-ignore -->
                  <span v-if="state.randomCocktail[`strMeasure${indx}`]">
                    <!-- @vue-ignore -->
                    {{ state.randomCocktail[`strMeasure${indx}`] }} -
                  </span>
                  <!-- @vue-ignore -->
                  {{ state.randomCocktail[`strIngredient${indx}`] }}
                </ion-label>
              </ion-item>
            </template>
          </ion-list>
        </ion-card-content>
      </ion-card>

    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';
import { reactive } from 'vue';
import axios from 'axios';

const state = reactive({
  randomCocktail: {},
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
  console.log(res);
}

const doRefresh = (event: CustomEvent) => {
  fetchRandomCocktail(false);
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
