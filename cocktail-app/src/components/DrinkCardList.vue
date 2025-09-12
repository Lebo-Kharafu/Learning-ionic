<template>
  <ion-list >
    <ion-item v-for="drink in drinkLst" :key=drink.idDrink >
      <DrinkCard :drink=drink />
    </ion-item>
  </ion-list>
</template>

<script setup lang="ts">
  import { Drink } from '@/types';
  import DrinkCard from "@/components/DrinkCard.vue";
  import {
    IonList,IonItem,
  } from '@ionic/vue';

  const props = defineProps<{
    drinkLst: Drink[],
  }>();

</script>

<style scoped>
  
</style>
