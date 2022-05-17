<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Claves</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-grid>
        <ion-row>
          <ion-col>
            <div>Claves</div>
          </ion-col>
          <ion-col>
            <div>Status</div>
          </ion-col>
        </ion-row>
        <ion-row v-for="(item,index) in listaClaves" :key="index">
          <ion-col>
            <div>{{listaKeys[index]}}</div>
          </ion-col>
          <ion-col>
            <div>{{item.status}}</div>
          </ion-col>
        </ion-row>
      </ion-grid>
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Tab 1</ion-title>
        </ion-toolbar>
      </ion-header>

      <ExploreContainer name="Hello World" />
    </ion-content>
  </ion-page>
</template>

<script >
import { defineComponent } from "vue";
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  IonGrid,
  IonRow,
  IonCol,
} from "@ionic/vue";
import ExploreContainer from "@/components/ExploreContainer.vue";
import { getDatabase, ref, onValue } from "firebase/database";

export default defineComponent({
  name: "Tab1Page",
  components: {
    ExploreContainer,
    IonHeader,
    IonToolbar,
    IonTitle,
    IonContent,
    IonPage,
    IonGrid,
    IonRow,
    IonCol,
  },
  mounted() {
    const db = getDatabase();
    const starCountRef = ref(db, "claves/");
    onValue(starCountRef, (snapshot) => {
      const data = snapshot.val();
      console.log(data);
      var cont = 0;
      snapshot.forEach((element) => {
        this.listaKeys[cont] = element.key;
        this.listaClaves[cont] = element.toJSON();
        cont++;
      });
    });
    console.log("lista de claves", this.listaClaves);
  },
  data() {
    return {
      listaClaves: [{ status: "", usuario: "" }],
      listaKeys: [],
    };
  },
});
</script>
<style>
:root {
  --ion-safe-area-top: 20px;
  --ion-safe-area-bottom: 22px;
}

ion-col > div {
  background-color: #f7f7f7;
  border: solid 1px #ddd;
  padding: 10px;
}
</style>