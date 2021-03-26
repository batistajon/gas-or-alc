<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Random Drink</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content v-if="state.loading">
      <div class="loading-center">
        <ion-spinner color="primary"></ion-spinner>
      </div>
    </ion-content>
    <ion-content :fullscreen="true" v-else>
      <ion-refresher slot="fixed" @ionRefresh="doRefresh">
        <ion-refresher-content></ion-refresher-content>
      </ion-refresher>
      <DrinkCard :drink="state.randomCocktail"/>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import DrinkDetailsInterface from '../interfaces/DrinkDetailsInterface.vue';
import { 
  IonPage, 
  IonHeader, 
  IonToolbar, 
  IonTitle, 
  IonContent,
  IonSpinner,
  IonRefresher,
  IonRefresherContent
} from '@ionic/vue';
import DrinkCard from '@/components/DrinkCard.vue';
import { reactive } from 'vue'
import axios from 'axios'

export default  {
  name: 'Tab1',
  components: { 
    IonHeader, 
    IonToolbar, 
    IonTitle, 
    IonContent, 
    IonPage, 
    DrinkCard,
    IonSpinner,
    IonRefresher,
    IonRefresherContent 
  },
  setup() {
    /* eslint-disable */ 
    const state = reactive({
      randomCocktail: {} as typeof DrinkDetailsInterface,
      loading: false
    });

    const fetchRandomCocktail = async(dispLoaderPage: boolean) => {
      
      if (dispLoaderPage) {
        state.loading = true;
      }

      const res = await axios.get('https://www.thecocktaildb.com/api/json/v1/1/random.php')

      if (res.data) {
        state.randomCocktail = res.data?.drinks[0];
      }

      state.loading = false;
    }

    const doRefresh = (event: CustomEvent): void => {
      fetchRandomCocktail(false)

      // @ts-ignore
      event.target?.complete()
    }

    fetchRandomCocktail(true);

    return {
      state, fetchRandomCocktail, doRefresh
    }
  }
}
</script>
<style scoped>
  .loading-center {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 90vh;
  }

  ion-spinner {
    transform: scale(1.5);
  }
</style>