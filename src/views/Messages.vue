<template>
  <div class="about container mt-5 pt-5">
    <h1>Liste des messages</h1>
    <table class="table table-responsive-md mt-5 pt-5">
      <thead>
      <router-link class="btn btn-dark" to="/nouveau-message" style="display: flex; justify-content: center;">Nouveau message</router-link>

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
          <router-link class="btn btn-outline-secondary" :to="{name:'message_edit', params: {id: message.id}}" >
            Editer
          </router-link>
          <button type="button" class="btn-close" aria-label="Close" @click="deleteMessage"></button>
        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import {deleteMessage, getMessages} from '../api/message'
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
  async deleteMessage() {
    await deleteMessage(this.id).then((response) => {
      console.log(response)
      //this.confirm = true //activation d'un bandeau de confirmation
      this.$router.push('/messages') //redirection
    })
  },
  mixins: [formatDateMixin],
  computed: {
    formattedDate() {
      return this.formatDate(this.date);
    }
  }
}
</script>
