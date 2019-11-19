<template>
  <div class="Questionnaire container" id="main">
      <div class="row quest-div" v-if="questionData.length > 0">
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
        {{answer}}
        {{username}}
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
      nbQ: 0,
      questionData: [],
      answer: [],
      username: null
    }
  },
  created: function () {
    this.loadQuestion()
  },
  mounted () {
    if (localStorage.username) {
      this.username = localStorage.username
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
      let score = []
      for (let i = 0; i < 9; i++) {
        let right = this.answer[i] === this.questionData[i].trueVal
        let result = { 'id': i,
          'right': right }
        score.push(result)
      }
      console.log(score)
    },
    loadQuestion: function () {
      var db = new PouchDB('app-questionnaire-vue')
      // var url = 'http://127.0.0.1:5984/app-questionnaire-vue'
      db.get('ee5b4987da3042ea0ec39e743d001b1f').then((doc, err) => {
        let data = doc.questions
        console.log(data)
        this.questionData = data
      }).catch((err) => {
        console.log('ERRROR : ' + err)
      })
    }
  }
}
</script>
