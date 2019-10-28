<template>
  <div class="Questionnaire container" id="main">
      <div class="row quest-div">
        <div class="col-lg-12" >
          <h2>Question {{nbQ}}</h2>
          <br/>
          <p>La question ?</p>
        </div>
        <div class="col-lg-12">
          <input class="form-control" placeholder="Réponse ..."/>
        </div>
        <div class="col-lg-12 vbottom" >
          <button class="btn float-left" @click="precQuestion">Précédent</button>
          <button class="btn float-right" @click="nextQuestion">Suivant</button>
        </div>
      </div>
  </div>
</template>
<script>
import PouchDB from 'pouchdb'

export default {
  data () {
    return {
      nbQ: 1,
      questionData: ''
    }
  },
  created: function () {
    this.loadQuestion()
  },
  methods: {
    nextQuestion: function () {
      console.log('nextQuestion')
    },
    precQuestion: function () {
      console.log('precQuestion')
    },
    loadQuestion: function () {
      var db = new PouchDB('app-questionnaire-vue')
      // var url = 'http://127.0.0.1:5984/app-questionnaire-vue'
      db.get('my-doc').then(function (doc) {
        console.log(doc)
      })
      console.log('loadQuestion')
    }
  }
}
</script>
