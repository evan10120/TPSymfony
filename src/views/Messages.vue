<template>
  <div class="about container mt-5 pt-5">
    <h1>Liste des messages</h1>
    <table class="table table-responsive-md mt-5 pt-5">
      <thead>
      <tr>
        <th>Id</th>
        <th>Titre</th>
        <th>Message</th>
        <th>Categorie</th>
        <th>Date</th>
        <th>Action</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="message in messages" :key="message.id">
        <td>
          {{message.id}}
        </td>
        <td>
          {{message.titre}}
        </td>
        <td>
          {{message.message}}
        </td>
        <td v-if="message.categorie">
          {{message.categorie.data.libelle}}
        </td>
        <td v-else>
          Aucune catégorie
        </td>
        <td>
          {{formatDate(message.date)}}
        </td>
        <td>
          <router-link class="btn btn-info" :to="{name:'message_edit', params: {id: message.id}}" >
            <font-awesome-icon icon="fa-solid fa-coffee" />
          </router-link>
        </td>
      </tr>
      </tbody>
    </table>
    <router-link class="btn btn-primary" to="/nouveau-message">Nouveau message</router-link>
  </div>
</template>

<script>
import {getMessages} from '../api/message'
import {getCategorie} from "@/api/categorie";
import formatDateMixin from '../plugin/formatDateMixin.js';

export default {
  name: 'Messages',
  data () {
    return {
      messages: null
    }
  },
  async mounted (){
    this.dataMessages()
  },
  methods: {
    async dataMessages() {
      this.messages = await getMessages().then((response) => {
        const data = response.data['hydra:member']
        data.forEach(async (message) => {
          if (message.categorie) {
            const categorieId = message.categorie.substr(-1)
            message.categorie = await getCategorie(categorieId)
          }
        })
        return data
      })
    },
  },
  mixins: [formatDateMixin],
  computed: {
    formattedDate() {
      return this.formatDate(this.date);
    }
  }
}
</script>
