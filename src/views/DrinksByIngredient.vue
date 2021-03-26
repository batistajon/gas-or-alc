<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
          <ion-buttons slot="start">
              <ion-back-button></ion-back-button>
          </ion-buttons>
        <ion-title>{{ ingredient }} Drinks</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content v-if="state.loading">
      <div class="loading-center">
        <ion-spinner color="primary"></ion-spinner>
      </div>
    </ion-content>
    <ion-content :fullscreen="true" v-else>
      <ion-list>
        <ion-item v-for="drink in state.lstDrinks" 
          :key="drink.idDrink"
          @click="
          () => 
            router.push(`/drink/${drink.idDrink}`)
          "
        >
          <ion-avatar slot="start">
            <img :src="drink.strDrinkThumb" />
          </ion-avatar>
          <ion-label>
            <h2>{{ drink.strDrink }}</h2>
          </ion-label>
        </ion-item>
      </ion-list>
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
  IonAvatar,
  IonSpinner,
  IonLabel,
  IonItem,
  IonList
} from '@ionic/vue';
import { reactive } from 'vue';
import { useRouter, useRoute } from 'vue-router';
import axios from 'axios';
import DrinkInterface from '../interfaces/DrinkInterface.vue'

export default  {
  name: 'DrinksByIngredient',
  components: { 
    IonHeader, 
    IonToolbar, 
    IonTitle, 
    IonContent, 
    IonPage, 
    IonButtons, 
    IonBackButton,
    IonAvatar,
    IonSpinner,
    IonLabel,
    IonItem,
    IonList
  },
  setup() {
    /* eslint-disable */
    const router = useRouter();
    const route = useRoute();
    const ingredient = route.params.ingredient as string;

    const state = reactive({
      lstDrinks: [] as typeof DrinkInterface[],
      loading: false
    })

    const fetchDrinksByIngredient = async (ingredient: string) => {
      state.loading = true;

      const res = await axios.get(`https://www.thecocktaildb.com/api/json/v1/1/filter.php?i=${ingredient}`);

      if (res.data) {
        state.lstDrinks = res.data?.drinks;
      }

      state.loading = false;
    };

    fetchDrinksByIngredient(ingredient);

    return {
      router,
      state,
      ingredient,
    }
  }
}
</script>
<style>
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