<template>
  <button class="clear" @click="clearScore() ">Clear Score</button>
  <ScoreBoard :win="this.win_count" :lose="this.lose_count" />
<div>
  <template v-if="this.question">
    <div class="tudo">
  <h1 v-html="question" />
  <div class="flexItens">
    <div v-bind:key="item" v-for="item in this.answers">
      <input  :disabled="this.answerSubmitted"  type="radio" :id="item" name="options" :value="item" v-model="this.selected_option">
      <label :for="item">{{item}}</label>
    </div>
    <button v-if="!answerSubmitted" @click="submitAnswer();" type="button">Send</button>

    <section v-if="answerSubmitted"  class="result">
      <h4 v-if="this.selected_option == this.correct">&#9989; Parabéns, a resposta "{{this.correct}}" está correta.</h4>
      <h4 v-else >&#10060;  Que pena, a resposta {{this.selected_option}} está errada. A resposta correta é {{this.correct}}.</h4>
      <button @click="getNewQuestion() ">Next</button>
    </section>
   
  </div>
</div>
</template>

</div>

</template>

<script>


import ScoreBoard from './components/ScoreBoard.vue'


export default {
  name: 'App',
  components: {
    ScoreBoard
  },
  data() {
    return {
      question: undefined,
      incorrects: [],
      correct: '',
      selected_option: undefined,
      answerSubmitted: false,
      lose_count: 0,
      win_count: 0
    }
  },
  computed: {
    answers (){
      let answers = JSON.parse(JSON.stringify(this.incorrects) ) 
      answers.splice(Math.round(Math.random()* answers.length), 0, this.correct)
      return answers
    }
  },  
  methods: {
    submitAnswer() {
      if (!this.selected_option) {
        alert('Pick one of the options');
      } else {
        this.answerSubmitted = true;
        if (this.selected_option == this.correct) {
          this.win_count++
        } else {
          this.lose_count++
        }
      }
    },

    getNewQuestion() {
      this.answerSubmitted = false;
      this.selected_option = undefined;
      this.question = undefined;

      this.axios.get('https://the-trivia-api.com/api/questions?limit=1').then(async (response)=> {
      console.log(response.data[0])
      this.question   =  response.data[0].question;
      this.incorrects =  response.data[0].incorrectAnswers;
      this.correct    =  response.data[0].correctAnswer;
    })
    },
    clearScore () {
            this.win_count = 0;
            this.lose_count = 0;
             }
  },  
  created() {
    this.getNewQuestion()
    this.lose_count = window.localStorage.getItem('loses') ? JSON.parse((window.localStorage.getItem('loses'))) : this.lose_count;
    this.win_count = window.localStorage.getItem('wins') ? JSON.parse((window.localStorage.getItem('wins'))) : this.win_count;
  },

  updated() {
   
    window.localStorage.setItem('wins', JSON.stringify(this.win_count))
    window.localStorage.setItem('loses', JSON.stringify(this.lose_count))
  }

}
</script>

<style lang="scss">


body{
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(92, 164, 236, 0.445);
}


#app {
  max-width: 960px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;


  .flexItens{
    display: flex;
    flex-direction: column;
    align-items: center;
  }


  input[type=radio] {
  margin: 12px 4px;
  }


}

  button {
    height: 40px;
    width: 120px;
    padding: 0 16px;
    color: white;
    background-color: blueviolet;
    border: 1px solid rgb(147, 16, 253);
    cursor: pointer;
  }



</style>
