<template>
  <div class="about mt-5 pt-5 container">
    <h1>Modification de la categorie</h1>
    <div v-if="confirm">
      Catégorie modifié avec succès !
    </div>
    <div>
      <label class="form-label" for="libelle">Libellé</label>
      <input class="form-control" type="text" name="libelle" v-model="libelle"  id="libelle"/>
    </div>
    <br>
    <div>
      <label class="form-label" for="couleur">Couleur</label>
      <input class="form-control" type="text" name="couleur" v-model="couleur"  id="couleur"/>
    </div>
    <br>
    <div>
      <button class="btn btn-primary" @click="editCategorie">Ajouter</button>
    </div>
    <br>
    <div>
      <button class="btn btn-danger" @click="deleteCategorie">Supprimer</button>
    </div>
    <br>
    <a class="btn btn-dark" href="/categories">Retour</a>
  </div>
</template>

<script>

import {putCategorie, getCategorie, deleteCategorie} from '../api/categorie'

export default {
  name: 'CategorieEdit',
  data () {
    return {
      id: '',
      couleur: '',
      libelle: '',
      confirm: false
    }
  },
  async mounted() {
    this.id = this.$route.params.id
    await getCategorie(this.id).then(response => {
      this.libelle = response.data.libelle
      this.couleur = response.data.couleur
    })
  },
  methods: {
    async editCategorie() {
      await putCategorie(this.id,{
        couleur: this.couleur,
        libelle: this.libelle
      }).then(() => {
        //this.confirm = true //activation d'un bandeau de confirmation
        this.$router.push('/categories') //redirection
      })
    },
    async deleteCategorie() {
      await deleteCategorie(this.id).then((reponse) => {
        console.log(reponse)
        //this.confirm = true //activation d'un bandeau de confirmation
        this.$router.push('/categories') //redirection
      })
    }
  }
}

</script>
