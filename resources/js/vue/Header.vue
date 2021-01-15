<template>
    <ul class="nav">
        <li class="nav-item">
            <a class="nav-link active" href="#">Quiz With A Bee And Itlog <b>QUIZ BEETLOG!</b></a>
        </li>
        <li class="nav-item">
            <a class="nav-link" href="#">Question {{questionIndex+1}}/{{totalQuestions}}</a>
        </li>
        <li class="nav-item">
            <a class="nav-link" href="#">Correct Answers: <b>{{numCorrectAnswers === 0 ? "None" : numCorrectAnswers}}</b></a>
        </li>
    </ul>
</template>

<script>
export default {
    name: "Header",
    props: {
        questionIndex: Number,
        totalQuestions: Number,
        numCorrectAnswers: Number,
    }
}
</script>

<style scoped>

</style>
