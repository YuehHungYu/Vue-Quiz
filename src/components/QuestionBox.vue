<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template slot="lead">
        {{currentQuestion.question }}
      </template>
      <hr class="my-4">
      <b-list-group>
        <b-list-group-item
        v-for="(answer,index) in shuffleAnswers" :key="index"
        @click="SelectAnswer(index)"
        :class="answerClass(index)"
        >
        <!-- :class="[!answered && selecedIndex===index?'selected':
          answered&&correctIndex===index?'correct':
          answered&&selecedIndex===index&&correctIndex!==index?'incorrect':''
          ]"  變成 Vue method -->
          {{ answer }}
        </b-list-group-item>
      </b-list-group>
      

      <b-button 
        variant="primary"
        @click="submitAnswer"
        :disabled="selecedIndex===null ||answered"
      >
        Submit
      </b-button>
      <b-button @click="next" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'
export default{
  props:{
    currentQuestion:Object,
    next:Function,
    increment:Function
  },
  data:function(){
    return{
      selecedIndex:null,
      correctIndex:null,
      shuffleAnswers:[],
      answered:false
    }
  },
  computed:{
    answers(){
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
      return answers
      // 這裡的answers在資料0-2錯誤3正確
    }
  },
  watch:{
    currentQuestion:{
      immediate:true,
      handler(){
        this.selecedIndex=null
        this.answered=false
        this.ShuffleAnswers()
      }
    }
  },
  methods:{
    SelectAnswer(index){
      this.selecedIndex=index
    },
    submitAnswer(){
      let isCorrect =false

      if(this.selecedIndex===this.correctIndex){
        isCorrect=true
      }
      this.answered=true
      this.increment(isCorrect)
    },
    ShuffleAnswers(){
      let answers = [...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer]
      this.shuffleAnswers=_.shuffle(answers)
      // 創造一個被打亂的集合(lodash.shuffle)
      this.correctIndex=this.shuffleAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    answerClass(index){
      let answerClass=''
      if(!this.answered && this.selecedIndex===index){
        answerClass='selected'
      }else if(this.answered&&this.correctIndex===index){
        answerClass='correct'
      }else if(this.answered&&this.selecedIndex===index&&this.correctIndex!==index){
        answerClass='incorrect'
      }
      return answerClass
    }
  }
}
</script>

<style scope>
  .list-group{
    margin-bottom: 20px;
  }
  .list-group-item{
    background: #EEE;
  }
  .list-group-item:hover{
    background: lightpink;
    cursor: pointer;
  }
  .btn{
    margin: 0 10px;
  }
  .selected{
    background: lightblue;
  }
  .correct{
    background: lightgreen;
  }
  .incorrect{
    background: red;
  }
</style>