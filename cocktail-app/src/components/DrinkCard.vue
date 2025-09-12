<template>
    <ion-card class="card-center">
        <img :src="drink.strDrinkThumb" />
        <ion-card-header>
            <ion-card-subtitle>
                {{ drink.strCategory }} | Served In
                {{ drink.strGlass }}
            </ion-card-subtitle>
            <ion-card-title>
                {{ drink.strDrink }}
            </ion-card-title>
        </ion-card-header>
        <ion-card-content>
            <p>
                {{ drink.strInstructions }}
            </p>
            <ion-list-header>
                Ingredients
            </ion-list-header>
            <ion-list>
                <template v-for="indx in 15" :key="indx">
                    <!-- @vue-ignore -->
                    <ion-item v-if="drink[`strIngredient${indx}`]">
                        <ion-label>
                            <!-- @vue-ignore -->
                            <span v-if="drink[`strMeasure${indx}`]">
                                <!-- @vue-ignore -->
                                {{ drink[`strMeasure${indx}`] }} -
                            </span>
                            <!-- @vue-ignore -->
                            {{ drink[`strIngredient${indx}`] }}
                        </ion-label>
                    </ion-item>
                </template>
            </ion-list>
        </ion-card-content>
    </ion-card>
</template>

<script setup lang="ts">

    import {
        IonCard, IonCardSubtitle, IonCardContent, IonCardHeader,
        IonCardTitle, IonItem, IonLabel, IonList, IonListHeader,
    } from '@ionic/vue';

    import { Drink } from "../types/index";
    defineProps<{ drink: Drink }>();
</script>

<style lang="css">
     .card-center {
            display: grid;
            align-items: center;
            text-align: center;
            justify-content: space-around;
            margin: 0;
        }

        img {
            display: block;
            margin: auto;
            width: 80%;
        }
</style>
