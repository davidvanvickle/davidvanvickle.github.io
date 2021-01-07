<template>
    <div class="my-6 w-6/12 mx-auto">
        <h2 class="mt-6">Quiz</h2>
        <div v-if="answeringQuestions" class="mt-6 shadow-xl rounded-md px-12 py-6">
            <h4 class="question my-6">{{ question }}</h4>
            <ul class="answers my-6 text-left border-t-2 pt-12">
                <li><a href="#" @click="selectAnswer('A')">A. {{ answers.A }}</a></li>
                <li><a href="#" @click="selectAnswer('B')">B. {{ answers.B }}</a></li>
                <li><a href="#" @click="selectAnswer('C')">C. {{ answers.C }}</a></li>
                <li><a href="#" @click="selectAnswer('D')">D. {{ answers.D }}</a></li>
            </ul>
            <p>{{ pagination }}</p>
        </div>
        <div v-if="!answeringQuestions" class="my-6 shadow-xl rounded-md px-12 py-6">
            <h3>Results</h3>
            <div>Correct: {{ correct.length }}</div>
            <div>Incorrect: {{ incorrect.length }}</div>
            <div>
                <button class="btn" @click="tryAgain()">Try Again</button>
            </div>
            <p class="mb-6">{{ resultMessage }}</p>
            <div v-for="q in trivia.questions" :key="q.q" class="text-left mb-3">
                {{ q.q }}<br>
                <strong>{{ q.mc[q.a] }}</strong>
            </div>
        </div>
    </div>
</template>

<script>
import trivia from '../data/trivia.json';
export default {
    name: 'Quiz',
    data () {
        return {
            answeringQuestions: true,
            trivia,
            question: '',
            answers: {A:'',B:'',C:'',D:''},
            answer: '',
            correct: [],
            incorrect: [],
            currentQuestion: -1
        }
    },
    created () {
        this.nextQuestion();
    },
    computed: {
        pagination () {
            return (this.currentQuestion + 1) + ' of ' + this.trivia.questions.length;
        },
        resultMessage () {
            if (this.correct.length < 5) {
                return 'Why not try again?';
            } else if (this.correct.length < 10) {
                return 'Well, I have seen worse.';
            } else {
                return 'You are a genius!';
            }
        }
    },
    methods: {
        selectAnswer (a) {
            let userAnswer = Object.assign({}, this.trivia.questions[this.currentQuestion]);
            userAnswer.a_user = a;
            if (this.trivia.questions[this.currentQuestion].a == a) {
                this.correct.push(userAnswer);
            } else {
                this.incorrect.push(userAnswer);
            }
            this.nextQuestion();
        },
        nextQuestion () {
            this.currentQuestion++;
            if (this.currentQuestion >= this.trivia.questions.length) {
                this.results();
                return;
            }
            this.question = this.trivia.questions[this.currentQuestion].q;
            this.answers = this.trivia.questions[this.currentQuestion].mc;
            this.answer = '';
        },
        results () {
            this.answeringQuestions = false;
        },
        tryAgain () {
            this.correct = [];
            this.incorrect = [];
            this.currentQuestion = -1;
            this.answeringQuestions = true;
            this.nextQuestion();
        }
    }
}
</script>

<style>
ul {
  list-style-type: none;
}

</style>

