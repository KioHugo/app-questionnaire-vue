<template>
  <div class="container">
    <div class="text-center bg-light rounded p-3">
          <h2 f-black> Espace admin </h2>
          <div v-if="users.length > 0">
            <div v-for="user in users" v-bind:key='user.identifiant'>
              Utilisateur : {{user.identifiant}}
              <div v-for="result in user.result" v-bind:key='result'>
                Score de : {{result.score}}
              </div>
            </div>
          </div>
    </div>
  </div>
</template>

<script>
import PouchDB from 'pouchdb'

var db = new PouchDB('app-questionnaire-vue')
var url = 'http://127.0.0.1:5984/app-questionnaire-vue'
db.replicate.from(url)

export default {
  data () {
    return {
      identifiant: null,
      users: []
    }
  },
  created: function () {
    this.loadUsers()
  },
  mounted () {
    if (localStorage.username && localStorage.username === 'HCaillaud') {
      this.identifiant = localStorage.username
    } else {
      this.$router.push('/')
    }
  },
  methods: {
    loadUsers: function () {
      db.get('Users').then((doc, err) => {
        let data = doc.users
        this.users = data
      }).catch((err) => {
        console.error(err)
      })
    }
  }
}
</script>
