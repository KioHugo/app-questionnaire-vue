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
        identifiant: this.prenom[0] + this.nom,
        nom: this.nom,
        prenom: this.prenom,
        societe: this.societe
      }
      // On récupère le doc
      db.get(`Users`).then((doc, err) => {
        // Si l'utilisateur n'existe pas encore
        if (!this.existUser(user.identifiant, doc)) {
          // On ajoute nos données
          doc.users.push(user)
          // On l'ajoute dans DB
          db.put(doc)
          // On sauvegarde
          db.replicate.to(url)
        }
      }).catch((err) => {
        console.error(err)
      })
      localStorage.username = this.prenom[0] + this.nom
    },
    existUser: function (identifiant, doc) {
      let found = false
      for (let idUser in doc.users) {
        let user = doc.users[idUser]
        if (user.identifiant === identifiant) {
          found = true
        }
      }
      return found
    }
  }
}
</script>
