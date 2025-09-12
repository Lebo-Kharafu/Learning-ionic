<template>
  <ion-avatar slot="start">
    <img :src="ingredientImage(img)">
  </ion-avatar>
  <ion-label>
    <h2>{{ title }}</h2>
  </ion-label>
</template>

<script setup lang="ts">
import {
   IonAvatar,IonLabel,
  } from '@ionic/vue';

  defineProps<{
    title: String,
    img: String,
  }>();

  const ingredientImage = (img: String) => {
    return `https://www.thecocktaildb.com/images/ingredients/${img}-small.png`
    // return `www.thecocktaildb.com/images/ingredients/${encodeURI(ingredient)}-small.png`
  }
</script>

<style scoped></style>
