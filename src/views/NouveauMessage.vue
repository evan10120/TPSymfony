<template>
  <div class="about container mt-5 pt-5">
    <h1>Ajout d'un message</h1>
    <div v-if="confirm">
      Message ajouté avec succès !
    </div>
    <div>
      <label class="form-label" for="titre">Titre</label>
      <input class="form-control" type="text" name="titre" v-model="titre"  id="titre"/>
    </div>
    <br>
    <div>
      <label class="form-label" for="message">Message</label>
      <input class="form-control" type="text" name="message" v-model="message"  id="message"/>
    </div>
    <br>
    <div>
      <label class="form-label" for="categorie">Categorie</label>
      <select class="form-select" name="categorie" v-model="selected" id="categorie">
        Categorie
        <option v-for="categorie in categories" :key="categorie.id" :value="categorie.id">
          {{categorie.libelle}}
        </option>
      </select>
    </div>
    <br>
    <div>
      <button class="btn btn-primary" @click="ajoutMessage">Ajouter</button>
    </div>
    <br>
    <a class="btn btn-info float-start" href="/messages">Retour</a>
  </div>
</template>

<script>

import {postMessage} from '../api/message'
import {getCategorie, getCategories} from "@/api/categorie";

export default {
  name: 'NouveauMessage',
  data () {
    return {
      message: '',
      titre: '',
      categories: {},
      categorie: {},
      selected: '',
      confirm: false
    }
  },
  async mounted () {
    this.categories = await getCategories().then((response) => {
      return response.data['hydra:member']
    })
  },
  methods: {
    async ajoutMessage() {
      await this.getCategorieById().then((data) => {
        postMessage({
          message: this.message,
          titre: this.titre,
          date: new Date(),
          categorie: data
        }).then((response) => {
          console.log(response)
          this.$router.push('/messages')
        })
      })
    },
    async getCategorieById () {
      return await getCategorie(this.selected).then((response) => {
        return response.data['@id']
      })
    }
  }
}

</script>
