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
            <button class="btn float-right login_btn" @click="addUser()">Commencer</button>
          </div>
      </div>
      <div v-if="result === false" class="error">Vous n'avez pas rempli tous les champs du formulaire</div>
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
      nom_err: false,
      prenom: '',
      prenom_err: false,
      societe: '',
      societe_err: false,
      result: null
    }
  },
  methods: {
    // Ajoute un nouvel utilisateur + connecte la personne
    addUser: function () {
      // On supprimme le user en mémoire
      delete localStorage.username
      // Si le formulaire n'est pas bien complété
      if (!this.verifForm()) {
        return false
      } else {
        // Object user
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
        // On stock le login
        localStorage.username = this.prenom[0] + this.nom
        // On envoie l'utilisateur sur le questionnaire
        this.$router.push('/questionnaire')
      }
    },
    // Vérifie que l'utilisateur à bien saisi toutes les informations de connexion
    verifForm: function () {
      this.result = true
      if (this.nom.trim() === '') {
        this.result = false
        this.nom_err = true
      } else { this.nom_err = false }

      if (this.prenom.trim() === '') {
        this.result = false
        this.prenom_err = true
      } else { this.prenom_err = false }

      if (this.societe.trim() === '') {
        this.result = false
        this.societe_err = true
      } else { this.societe_err = false }
      return this.result
    },
    // True si l'utilisateur existe false sinon
    existUser: function (identifiant, doc) {
      let found = false
      // TODO remplacer par un find ?
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
