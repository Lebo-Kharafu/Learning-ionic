<template>
  <ion-avatar slot="start">
    <img :src="DrinkImage(img)">
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

  const DrinkImage = (img: String) => {
    return `${img}/small`
  }
</script>

<style scoped></style>
