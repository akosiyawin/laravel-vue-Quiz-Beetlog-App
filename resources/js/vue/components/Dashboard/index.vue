<template>
   <div id="app__wrapper">
       <Header
        :questionIndex="questionIndex"
        :totalQuestions="questions.length"
        :numCorrectAnswers="numCorrectAnswers"
       />
        <div class="container mt-5">
            <div class="alert alert-success" role="alert" v-if="!questions.length">
                <h4 class="alert-heading">Please wait a second while we are loading your questions!</h4>
            </div>
        </div>
       <QuestionBox :incrementCorrectAnswers="incrementCorrectAnswers"
                    :isLastQuestion="isLastQuestion"
                    :totalQuestions="questions.length"
                    :numCorrectAnswers="numCorrectAnswers"
                    :restartGame="initiateQuestions"
                    :next="next" v-if="questions.length" :question="questions[questionIndex]"></QuestionBox>
   </div>
</template>

<script>
import Header from "../../Header";
import QuestionBox from "../../QuestionBox";
export default {
    name: "index",
    data(){
        return{
            questions: [],
            questionIndex: 0,
            numCorrectAnswers: 0,
            isLastQuestion: false
        }
    },
    components: {
        Header,
        QuestionBox
    },
    mounted() {
        this.initiateQuestions()
    },
    methods: {
        next(){
            if(this.questionIndex !== this.questions.length-1){
                this.questionIndex++
            }

            if(this.questionIndex === this.questions.length-1) this.isLastQuestion = true
        },
        incrementCorrectAnswers(){
            this.numCorrectAnswers++
        },
        initiateQuestions(){
            this.questionIndex = 0
            this.numCorrectAnswers = 0
            this.isLastQuestion = false
            this.questions = []
            const data = fetch('https://opentdb.com/api.php?amount=10&category=23&type=multiple',{method:"get"})
            data.then(r=>{
                return r.json()
            })
            .then(jsonData=>{
                this.questions = jsonData.results
            })
        }
    }
}
</script>

<style scoped>

</style>
