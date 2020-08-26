<template>
    <div id="question" v-bind:class="{ important: questionData.needsAnswer, correct: questionData.isCorrect, wrong: !questionData.isCorrect  }">
        <div class="header">
            {{ questionData.qNum }}. {{ questionData.question }}
        </div>
        <section>
            <label 
                v-bind:for="questionData.qNum" 
                class="option" 
                v-for="(item, index) in questionData.options"
                v-bind:class="{ rightAnswer: index === questionData.actualAnswer }"  
                :key="index">
                <input 
                    type="radio" 
                    :name="questionData.qNum" 
                    v-model="questionData.userAnswer" 
                    :value="index" 
                    @change="onSelect" 
                    :checked="questionData.userAnswer === index"
                />
                {{ item }}
            </label>
        </section>
        <div v-if="questionData.isCorrect" class="status good">
            Correct
        </div>
        <div v-else class="status bad">
            Wrong
        </div>
    </div>
</template>

<script>
export default {
    name: "Question",
    props: {
        questionData: Object
    },
    methods: {
        onSelect(event) {
            console.log(this.questionData);
            this.$emit('change', event.target.value);
        }
    }
}
</script>

<style scoped>

/* Base Styles */
#question {
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    max-width: 75rem;
    width: calc(100% - 20rem);
    min-width: 20rem;
    border-radius: 20px;
    border: 0.1rem solid transparent;
    padding: 2rem;
    margin: 0 auto 2rem auto;
    box-shadow: 3px 3px 15px 3px #ccc;
}

.header {
    text-align: left;
    margin-bottom: 1rem;
}

section {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    width: 70%;
    padding: 0;
    margin: 0;
}

.option {
    display: flex;
    flex-direction: row;
    align-items: center;
    width: 100%;
    text-align: left;
    border-radius: 5px;
    padding: 0.5rem;
}

input {
    margin: 0 0.5rem 0 0;
}

.status {
    position: absolute;
    right: 2rem;
    bottom: 2rem;
    display: none;
}

/* When user clicks submit but forgets to answer at least 1 question */
#question.important {
    border-color: yellow;
}

/* When the exam has been successfully graded */
#app.graded #question.correct {
    border-color: green;
}

#app.graded #question.wrong {
    border-color: red;
}

#app.graded .status.bad {
    display: block;
    color: red;
}

#app.graded .status.good {
    color: green;
    display: block;
}

#app.graded #question.wrong .rightAnswer {
    color: white;
    background-color: #11AB07;
    border: 0.1rem solid #0A5905;

}
</style>