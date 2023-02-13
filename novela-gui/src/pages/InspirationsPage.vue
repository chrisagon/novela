<template>
  <q-page padding>
    <div class="row justify-center center-items">
      <div class="col-md-10 col-sm-12">
        <q-card class="">
          <q-toolbar class="bg-secondary text-white q-pa-sm">
            Créez votre inspiration ! (Tout est enregistré dans le livre d'inspiration)<q-space /><q-btn @click="makeInspiration"
              class="coloredbutton bg-white" :disabled="loading" label="Creation!"></q-btn>
          </q-toolbar>
          <q-card-actions class="q-pa-md">
            <q-input v-model="inspiration.kind" class="inputek" dense label="Kind of story? (fantasy)"
              ></q-input><q-input v-model="inspiration.somethingAbout" class="inputek" dense label="Histoire à propos de quoi ?"></q-input>
            <q-toggle class="inputek" label="Mode automatique complet (si désactivé - vous avez le contrôle total sur la génération avec l'histoire sur quel champ)" v-model="inspiration.fullAuto"></q-toggle>
          </q-card-actions>


          <q-card-section>
            <div class="text-h6"></div>
          </q-card-section>

          <q-card-section class="q-pt-none">
            <q-input readonly v-model="loaded.text" rows="20" type="textarea"></q-input>
          </q-card-section>
          <img v-if="loaded.image" :src="`/api/v1/images/get/${loaded.image}`">
          <q-inner-loading :showing="Laissez-moi reflechir">
            <q-spinner-ball size="50px" class="coloredbutton" />
          </q-inner-loading>
        </q-card>

      </div>
    </div>
  </q-page>
</template>

<style>
.inputek {
  width: 300px;
  margin: 6px;
}

.coloredbutton {
  color: #000;
  animation: color-change 3s infinite;
}

@keyframes color-change {
  0% {
    color: red;
  }

  25% {
    color: blue;
  }

  50% {
    color: green;
  }

  75% {
    color: purple;
  }

  100% {
    color: red;
  }
}
</style>
<script>
import { ref } from "vue";
export default {
  methods: {
    async makeInspiration() {
      this.loading = true
        let data = await this.$novelaAPI.createInspiration({
          somethingAbout: this.inspiration.somethingAbout,
          fullAuto: this.inspiration.fullAuto,
          length: 2048,
          kind: this.inspiration.kind
        })
        this.loaded = data.data
      this.loading = false
    }

  },
  data() {
    return {
      inspiration: ref({
        fullAuto: true
      }),
      loading: ref(false),
      loaded: ref({})
    }
  }
  // name: 'PageName',
}
</script>
