<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
          <ion-buttons slot="start">
              <ion-back-button></ion-back-button>
          </ion-buttons>
        <ion-title>{{ state.drink.strDrink  }}</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content v-if="state.loading">
      <div class="loading-center">
        <ion-spinner color="primary"></ion-spinner>
      </div>
    </ion-content>
    <ion-content :fullscreen="true" v-else>
     <DrinkCard :drink="state.drink"/>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { 
    IonPage, 
    IonHeader, 
    IonToolbar, 
    IonTitle, 
    IonContent,
    IonButtons,
    IonBackButton, 
} from '@ionic/vue';
import DrinkCard from '@/components/DrinkCard.vue'
import { reactive } from 'vue';
import { useRoute } from 'vue-router';
import axios from 'axios';
import DrinkDetailsInterface from '../interfaces/DrinkDetailsInterface.vue';

export default {
    name: 'Drink',
    components: { 
        IonPage, 
        IonHeader, 
        IonToolbar, 
        IonTitle, 
        IonContent, 
        DrinkCard,
        IonButtons,
        IonBackButton,  
    },
    setup() {
        /* eslint-disable */
        const route = useRoute();
        const drinkId = route.params.id as string;

        const state = reactive({
            drink: {} as typeof DrinkDetailsInterface,
            loading: false
        });

        const fetchDrinkById = async(drinkId: string) => {
            state.loading = true;

            const res = await axios.get(`https://www.thecocktaildb.com/api/json/v1/1/lookup.php?i=${drinkId}`);

            if (res.data) {
                state.drink = res.data?.drinks[0];
            }

            state.loading = false
        }

        fetchDrinkById(drinkId);

        return {
            state
        }
    }
}
</script>

<style>

</style>