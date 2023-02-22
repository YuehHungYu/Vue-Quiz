<template>
  <div id="app">
    <HeaderBox
      :numCorrect="numCorrect"
      :numTotal="numTotal"
    />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
          v-if="questions.length"
          :currentQuestion="questions[index]"
          :next="next"
          :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import QuestionBox from './components/QuestionBox.vue' 
import HeaderBox from './components/HeaderBox.vue' 

export default {
  name: 'App',
  components: {
    QuestionBox,
    HeaderBox
  },
  data(){
    return{
      questions:[],
      index:0,
      numCorrect:0,
      numTotal:0
    }
  },
  methods:{
    next:function(){
      this.index++
    },
    increment(isCorrect){
      if(isCorrect){
        this.numCorrect++
      }
      this.numTotal++
    }
  },
  mounted:function(){
    fetch('https://opentdb.com/api.php?amount=50',{
      method:'get'
    }).then(response=>{return response.json()}).then(json=>{
      this.questions=json.results
    })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.bv-example-row{
  background: lightcyan
}
</style>
