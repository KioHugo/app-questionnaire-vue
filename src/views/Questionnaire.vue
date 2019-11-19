<template>
  <div class="Questionnaire container" id="main">
      <div class="row quest-div" v-if="questionData.length > 0 && !finished">
        <div class="col-lg-12" >
          <h2>Question {{nbQ + 1}}</h2>
          <br/>
          <p>{{questionData[nbQ].question}}</p>
        </div>
        <div class="col-lg-12">
          <div v-for="response in questionData[nbQ].reponses" v-bind:key='response.id'>
            <input type="radio" :id="response.id" :value="response.id" v-model="answer[nbQ]">
            <label :for="response.id">{{response.value}}</label>
          </div>
        </div>
        <div class="col-lg-12 vbottom" >
          <div v-if=" nbQ > 0 && nbQ <= 9">
            <button class="btn float-left" @click="precQuestion">Précédent</button>
          </div>
          <div v-if=" nbQ < 9 && nbQ >= 0">
            <button class="btn float-right" @click="nextQuestion">Suivant</button>
          </div>
          <div v-if="nbQ == 9">
              <button class="btn btn-info float-right" @click="validerQuestion">Valider</button>
          </div>
        </div>
      </div>
      <div class="quest-div justify-content-center" v-if="finished" >
        <div> <h1>Vous avez terminé le questionnaire !</h1> </div>
        <div> Votre score est de : {{score}} </div>
      </div>
  </div>
</template>
<script>
import PouchDB from 'pouchdb'
import json from '../assets/question.json'

var db = new PouchDB('app-questionnaire-vue')
var url = 'http://127.0.0.1:5984/app-questionnaire-vue'
db.replicate.from(url)

export default {
  data () {
    return {
      nbQ: 0,
      questionData: json.questions,
      answer: [],
      username: null,
      finished: false,
      score: 0
    }
  },
  mounted () {
    if (localStorage.username && localStorage.username !== 'undefined' && localStorage.username !== undefined) {
      this.username = localStorage.username
    } else {
      this.$router.push('/')
    }
  },
  methods: {
    nextQuestion: function () {
      if (this.nbQ < 9) {
        this.nbQ += 1
      }
    },
    precQuestion: function () {
      if (this.nbQ > 0) {
        this.nbQ -= 1
      }
    },
    validerQuestion: function () {
      let score = {
        'result_list': [],
        'score': null
      }
      let scoreTotal = 0
      for (let i = 0; i < 9; i++) {
        let right = this.answer[i] === this.questionData[i].trueVal
        let result = { 'id': i,
          'right': right }
        if (right) scoreTotal++
        score.result_list.push(result)
      }
      score.score = scoreTotal
      // On récupère le doc
      db.get(`Users`).then((doc, err) => {
        doc.users.map((user) => {
          if (user.identifiant === this.username) {
            if (user.result === undefined) user.result = []
            user.result.push(score)
          }
        })
        // On l'ajoute dans DB
        db.put(doc).then(function (doc) { })
        // On sauvegarde
        db.replicate.to(url)
      })
      this.score = scoreTotal
      this.finished = true
    }
  }
}
</script>
