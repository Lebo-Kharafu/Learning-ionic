<template>
	<ion-page>
		<ion-header>
			<ion-toolbar>
				<ion-title>Search Drinks</ion-title>
			</ion-toolbar>
		</ion-header>
		<ion-content v-if="state.loading">
			<section class="loading-center">
				<ion-spinner color="primary"></ion-spinner>
			</section>
		</ion-content>
		<ion-content :fullscreen="true" v-else>
			<ion-searchbar
				:animated="true"
				:debounce="1000"
				placeholder="Search a Drink"
				:onIonChange="(event:CustomEvent) => handleInput(event.detail.value)"
			>
			</ion-searchbar>
			<DrinkCardList :drinkLst="state.lstDrink"/>
		</ion-content>
	</ion-page>
</template>

<script setup lang="ts">
	import {
		IonPage,
		IonHeader,
		IonToolbar,
		IonTitle,
		IonContent,
		IonSpinner,
		IonSearchbar,
	} from "@ionic/vue";
	import { Drink } from "@/types/index";
	import { reactive, ref } from "vue";
	import axios from "axios";
	import DrinkCardList from "@/components/DrinkCardList.vue";

	const state = reactive({
		lstDrink: [] as Drink[],
		loading: false,
	});

	let searchTerm = ref<string>("");

	const fetchDrinks = async () => {
		state.loading = true;

		if (searchTerm.value) {
			const res = await axios.get(
				`https://www.thecocktaildb.com/api/json/v1/1/search.php?s=${searchTerm.value}`
			);
			if (res.data) {
				state.lstDrink = res.data?.drinks;
			}
		} else {
			state.lstDrink = [];
		}
		console.log(state.lstDrink);
		state.loading = false;
	};

	const handleInput = (search: string) => {
		searchTerm.value = search;
		fetchDrinks();
	};

	fetchDrinks();
</script>

<style scoped>

</style>
