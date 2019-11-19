<template>
  <div class="container">
    <div class="card card-login mx-auto text-center bg-light">
      <div class="card-header mx-auto bg-light">
          <h2 f-black> S'enregistrer </h2>
      </div>
      <div class="card-body">
          <div class="input-group form-group">
            <div class="input-group-prepend">
              <span class="input-group-text"><i class="fas fa-user"></i></span>
            </div>
            <input type="text" name="nom" class="form-control" placeholder="Nom ..." v-model="nom">
          </div>

          <div class="input-group form-group">
            <div class="input-group-prepend">
              <span class="input-group-text"><i class="fas fa-user"></i></span>
            </div>
            <input type="text" name="prenom" class="form-control" placeholder="Prénom ..." v-model="prenom">
          </div>

          <div class="input-group form-group">
            <div class="input-group-prepend">
              <span class="input-group-text"><i class="fas fa-industry"></i></span>
            </div>
            <input type="text" name="societe" class="form-control" placeholder="Société ..." v-model="societe">
          </div>
          <div class="form-group">
            <router-link to="/questionnaire" ><button class="btn float-right login_btn" @click="addUser()">Commencer</button></router-link>
          </div>

      </div>
    </div>
  </div>
</template>

<script>
import PouchDB from 'pouchdb'

var db = new PouchDB('app-questionnaire-vue')
var url = 'http://127.0.0.1:5984/app-questionnaire-vue'

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data () {
    return {
      nom: '',
      prenom: '',
      societe: ''
    }
  },
  methods: {
    addUser: function () {
      var user = {
        _id: 'user - ' + this.prenom[0] + this.nom,
        nom: this.nom,
        prenom: this.prenom,
        societe: this.societe
      }
      db.put(user).then(function (doc) {
        console.log(doc)
      })
      localStorage.username = this.prenom[0] + this.nom
      db.replicate.to(url)
    }
  }
}
</script>
