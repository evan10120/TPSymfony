<template>
  <div class="about container mt-5 pt-5 text-center">
    <h1>Liste des catégories</h1>
    <table class="table table-responsive-md mt-5 pt-5">
      <thead>
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
          <router-link class="btn btn-info" :to="{name:'categorie_edit', params: {id: categorie.id}}">
            Modifier
          </router-link>
        </td>
      </tr>
      </tbody>
    </table>
    <router-link class="btn btn-primary" to="/nouvelle-categorie">Nouvelle catégorie</router-link>
    <router-view></router-view>
  </div>
</template>

<script>
import {getCategories} from '../api/categorie'

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

  }
}

</script>
