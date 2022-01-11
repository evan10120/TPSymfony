<template>
  <div class="about container mt-5 pt-5 text-center">
    <h1>Liste des catégories</h1>
    <table class="table table-responsive-md mt-5 pt-5">
      <thead>
      <router-link class="btn btn-dark" to="/nouvelle-categorie" style="display: flex; justify-content: center;">Nouvelle catégorie</router-link>
      <tr>
        <th>Id</th>
        <th>Libelle</th>
        <th>Couleur</th>
        <th>Action</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="categorie in categories" :key="categorie.id">
        <td>
          {{categorie.id}}
        </td>
        <td>
          <router-link :to="{name:'categorie_messages', params: {id: categorie.id}}">
            {{categorie.libelle}}
          </router-link>
        </td>
        <td>{{categorie.couleur}}</td>
        <td>
          <router-link class="btn btn-outline-secondary" :to="{name:'categorie_edit', params: {id: categorie.id}}">
            Editer
          </router-link>
          <button type="button" class="btn-close" aria-label="Close" @click="deleteCategorie"></button>
        </td>
      </tr>
      </tbody>
    </table>
    <router-view></router-view>
  </div>
</template>

<script>
import {deleteCategorie, getCategories} from '../api/categorie'

export default {
  name: 'Categories',
  data () {
    return {
      categories: null
    }
  },
  async mounted () {
    this.categories = await getCategories().then((response) => {
      return response.data['hydra:member']
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

</script>
