<template>
    <div class="list-group">
        <a v-for="(choice,index) in shuffledChoices"
           @click="selectAnswer(index)"
           :class="[index === selectedAnswerIndex && isAnswerCorrect === null ? 'active' :
           isAnswerCorrect && index === selectedAnswerIndex ? 'bg-success':
           !isAnswerCorrect && index === selectedAnswerIndex ? 'bg-danger' :
           choice === correctAnswer && !isAnswerCorrect && submittedAnswer ? 'bg-success' : '']"
           href="#" v-html="choice" class="list-group-item list-group-item-action"></a>
    </div>
</template>

<script>
import _ from 'lodash';

export default {
    name: "AnswersList",
    props: {
        choices: Array,
        setSelectedIndex: Function,
        setShuffledChoices: Function,
        submittedAnswer: String,
        correctAnswer: String,
        isAnswerCorrect: Boolean,
    },
    data(){
        return {
            selectedAnswerIndex: null,
            shuffledChoices: [],
        }
    },
    methods:{
        selectAnswer(index){
            if(this.submittedAnswer) return

            this.selectedAnswerIndex = index;
            this.setSelectedIndex(index)
        }
    },
    watch: {
        /*choices(){
            console.log("Alarh")
            this.shuffledChoices = _.shuffle(this.choices)
        }*/
        choices: { /*To invoke the first time this component run*/
            immediate: true,
            handler(){
                this.selectedAnswerIndex = null
                this.shuffledChoices = _.shuffle(this.choices)
                this.setShuffledChoices(this.shuffledChoices)
            }
        }
    },

}
</script>

<style scoped>

</style>
