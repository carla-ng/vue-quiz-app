<script setup>
    import { ref, computed } from 'vue'

    const questions = ref([
        {
            question: 'Which company created Mario?',
            answer: 0,
            options: [
                'Nintendo',
                'Square Enix',
                'Sega'
            ],
            selected: null
        },
        {
            question: 'What is the name of Mario\'s main villain?',
            answer: 2,
            options: [
                'Luigi',
                'Toad',
                'Bowser'
            ],
            selected: null
        },
        {
            question: 'What color is Peach\'s dress?',
            answer: 1,
            options: [
                'Red',
                'Pink',
                'Yellow',
                'Blue'
            ],
            selected: null
        }
    ])

    const quizCompleted = ref(false)
    const currentQuestion = ref(0)

    // Get user score
    const score = computed(() => {
        let value = 0
        questions.value.map( question => {
            if ( question.selected == question.answer ) value++
        })

        return value
    })

    // Get current question
    const getCurrentQuestion = computed(() => {
        let question = questions.value[ currentQuestion.value ]
        question.index = currentQuestion.value

        return question
    })

    // Set user selected answer
    const setAnswer = event => {
        questions.value[ currentQuestion.value ].selected = event.target.value
        event.target.value = null
    }

    // Go to next question
    const nextQuestion = () => {
        if ( currentQuestion.value < questions.value.length - 1 ) {
            currentQuestion.value++
        } else {
            quizCompleted.value = true
        }
    }

</script>


<template>

    <main>

        <h1>Super Mario Quiz</h1>

        <section v-if="!quizCompleted" class="quiz">
            <div class="quiz-info">
                <span class="question">{{ getCurrentQuestion.question }}</span>
                <span class="score">
                    <span>Score:</span> {{ score }} / {{ questions.length }}
                </span>
            </div>

            <div class="options">
                <label v-for="(option, index) in getCurrentQuestion.options"
                       :key="index"
                       :class="`option
                                ${ getCurrentQuestion.selected == index ? index == getCurrentQuestion.answer ? 'correct' : 'wrong' : '' }
                                ${ getCurrentQuestion.selected != null && getCurrentQuestion.selected != index ? 'disabled' : '' }
                `">
                    <input type="radio"
                           :name="getCurrentQuestion.index"
                           :value="index"
                           v-model="getCurrentQuestion.selected"
                           :disabled="getCurrentQuestion.selected"
                           @change="setAnswer"
                    >

                    <span>{{ option }}</span>
                </label>
            </div>

            <button @click="nextQuestion" :disabled="!getCurrentQuestion.selected">
                {{ 
                    getCurrentQuestion.index == questions.length - 1 ? 'Finish quiz' : getCurrentQuestion.selected == null ? 'Select an option' : 'Next question'
                }}
            </button>
        </section>

        <section v-else>
            <h2>Congrats! You have finished the quiz</h2>
            <p>Your score is {{ score }}/{{ questions.length }}</p>
        </section>

    </main>

</template>


<style lang="scss">
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Montserrat', sans-serif;
}

body {
    background-color: #e50113;

    #app {
        min-height: 100vh;
        padding: 2rem;

        main {
            display: flex;
            align-items: center;
            flex-direction: column;

            h1 {
                color: #FFF;
                margin-top: 1rem;
                margin-bottom: 2rem;
                text-align: center;
            }

            section {
                &.quiz {
                    background-color: #FFF;
                    border-radius: 20px;
                    box-shadow: rgba(0, 0, 0, 0.25) 0px 5px 10px;
                    padding: 1rem;
                    width: min(640px, 100%);

                    .quiz-info {
                        display: flex;
                        justify-content: space-between;

                        margin-bottom: 1rem;

                        .question {
                            color: rgb(30, 29, 92);
                            font-size: 1.25rem;
                            font-weight: 600;
                        }

                        .score {
                            color: rgb(30, 29, 92);
                            padding-right: 1rem;

                            span { font-weight: 600; }
                        }
                    }

                    .options {
                        margin-bottom: 1rem;

                        .option {
                            background-color: #2E6EEE;
                            border-radius: 0.5rem;
                            color: #FFF;
                            cursor: pointer;
                            display: block;
                            margin-bottom: 0.5rem;
                            padding: 1rem;

                            &.correct { background-color: #2CCE7D; }
                            &.wrong { background-color: #FF5A5F; }
                            &.disabled { opacity: 0.5; }

                            &:last-of-type { margin-bottom: 0; }

                            &:hover:not(.disabled):not(.correct):not(.wrong) { background-color: #6590E6; }

                            input { display: none; }
                        }
                    }

                    button {
                        appearance: none;
                        border: none;
                        outline: none;

                        background-color: #0054FF;
                        border-radius: 0.5rem;
                        color: #FFF;
                        cursor: pointer;
                        float: right;
                        font-size: 1.25rem;
                        font-weight: 700;
                        padding: 0.5rem 1rem;

                        &:disabled { opacity: 0.5; }
                    }
                }

                h2 {
                    font-size: 1.5rem;
                    margin-bottom: 2rem;
                    text-align: center;
                }

                p {
                    font-size: 1.25rem;
                    text-align: center;
                }
            }
        }
    }
}
</style>
