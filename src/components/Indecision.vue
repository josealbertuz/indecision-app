<template>
  <img v-if="image" :src="image" alt="gif">
  <div v-else class="black-container"></div>
  <div class="main-container">
    <h1>Ask something cool</h1>
    <input v-model="question" placeholder="Ask something"/>
    <p>Remember to end your question with a (?)</p>
    <p>{{ answer }}</p>
  </div>

</template>

<script>
export default {
  name: "Indecision",
  data() {
    return {
      question: '',
      image: '',
      answer: ''
    }
  },
  methods: {
    getAnswer(){
      this.answer = 'Loading...';
      fetch('https://yesno.wtf/api')
          .then(response => response.json())
          .then(({image, answer}) => {
            this.image = image;
            this.answer = answer;
          });
    }
  },
  watch: {
    question(newQuestion, oldQuestion) {
      if (!newQuestion.endsWith('?')) {
        this.image = '';
        return;
      }
      this.getAnswer();
    }
  }
}
</script>

<style scoped>
img {
  position: absolute;
  height: 100%;
  width: 100%;
  z-index: -1;
}
.black-container {
  position: absolute;
  background-color: #2d2d2d;
  height: 100%;
  width: 100%;
  z-index: -1;
}
input {
  font-size: inherit;
  width: 20em;
  padding: 0.5em 0.5em;
  border-radius: 0.4em;
}

.main-container {
  font-size: 1.2rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 1.5em;
  color: white;
}
</style>