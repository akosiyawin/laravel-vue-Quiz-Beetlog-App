<template>
    <div class="jumbotron">
      <div class="container">
          <div class="alert alert-success" v-if="isDone" role="alert">
              <h4 class="alert-heading">Well done!</h4>
              <p>
                  You have completed the Quizz Beetlog. Congratulations, you are now a certified Beetlogers!
              </p>
              <hr>
              <p class="mb-0">YOUR TOTAL SCORE IS <b>{{numCorrectAnswers}}/{{totalQuestions}}</b></p>
          </div>
          <h1 class="display-4">Answer the Question</h1>
          <p class="lead">Choose the best answer that you can think of good luck!</p>
          <hr class="my-4">
          <h5 v-html="question.question"></h5>
          <AnswersList :setSelectedIndex="setSelectedIndex"
                       :setShuffledChoices="setShuffledChoices"
                       :submittedAnswer="submittedAnswer"
                       :correctAnswer="question.correct_answer"
                       :isAnswerCorrect="isAnswerCorrect"
                       :choices="choices"   class="my-3"/>
          <p class="lead">
              <button class="btn btn-primary btn-lg"
                      :disabled="selectedIndex === null || submittedAnswer"
                      @click="submitAnswer"
                      v-if="!isDone"
                      role="button" >Submit</button>
              <button class="btn btn-success btn-lg"
                      v-if="!isDone"
                      :disabled="submittedAnswer === null || isLastQuestion" role="button" @click="nextQuestion">Next</button>
              <button class="btn btn-success btn-block btn-lg" @click="reloadPage" v-if="isDone">Retake Quiz</button>
          </p>
      </div>
    </div>
</template>

<script>
import AnswersList from "./AnswersList";
import _ from "lodash";
export default {
name: "QuestionBox",
    components: {
        AnswersList
    },
    props: {
        question: Object,
        next: Function,
        totalQuestions: Number,
        numCorrectAnswers: Number,
        incrementCorrectAnswers: Function,
        restartGame: Function,
        isLastQuestion: Boolean,

    },
    data(){
        return {
            selectedIndex: null,
            submittedAnswer: null,
            shuffledChoices: [],
            isAnswerCorrect: null,
            isDone: false
        }
    },
    methods: {
        setSelectedIndex(index){
          this.selectedIndex = index;
        },
        submitAnswer(){
            this.submittedAnswer = this.shuffledChoices[this.selectedIndex]
            if(this.question.correct_answer === this.submittedAnswer){
                this.incrementCorrectAnswers()
                this.isAnswerCorrect = true
            }else{
                this.isAnswerCorrect = false
            }

            if(this.isLastQuestion){
                this.isDone = true
            }
        },
        setShuffledChoices(choices){
            this.shuffledChoices = choices
        },
        resetIsAnswerCorrect(){
            this.isAnswerCorrect = null
        },
        nextQuestion(){
            this.resetIsAnswerCorrect()
            this.submittedAnswer = null
            this.selectedIndex = null
            this.next()
        },
        reloadPage(){
            this.selectedIndex = null
            this.submittedAnswer = null
            this.isAnswerCorrect = null
            this.isDone = null
            this.restartGame()
        }
    },
    computed: {
        choices(){
            const answers = [...this.question.incorrect_answers]
            answers.push(this.question.correct_answer)
            return answers
        }
    }
}
</script>

<style scoped>

</style>
