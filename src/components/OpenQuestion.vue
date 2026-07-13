<template>
  <div id="open-question">
    <div class="question-container">

      <div class="question-header">
        <h2 class="question-title">{{ title }}</h2>
        <p class="question-subtitle">{{ subtitle }}</p>
      </div>

      <textarea
        v-model="userAnswer"
        class="answer-box"
        placeholder="כתבו את תשובתכם כאן..."
        :disabled="submitted"
      ></textarea>

      <button
        class="submit-btn"
        @click="submitAnswer"
        :disabled="submitted || userAnswer.trim() === ''"
      >
        הגש תשובה
      </button>

      <div
        class="result-message"
        v-if="submitted"
        :class="{ correct: isCorrect, wrong: !isCorrect }"
      >
        <span v-if="isCorrect">✔ תשובה נכונה!</span>
        <span v-else>✖ התשובה אינה מלאה.</span>
      </div>

      <button
        v-if="canContinue"
        class="move-on"
        @click="handleMoveOn"
      >
        המשך
      </button>

      <!-- Popup -->
      <div
        class="popup-overlay"
        v-if="showPopup"
      >
        <div class="popup">

          <button
            class="close-btn"
            @click="closePopup"
          >
            ✕
          </button>

          <h3 v-if="isCorrect === false">בואו נקרא את התשובה המדויקת </h3>
          <h3 v-if="isCorrect === true">  נכון! בכל מקרה שתדעו-</h3>
          <p>{{ correctAnswer }}</p>

        </div>
      </div>

    </div>
  </div>
</template>

<script>
import json from "../../text.json";

export default {
  name: "OpenQuestion",

  props: ["questionNum"],

  data() {
    return {
      userAnswer: "",
      submitted: false,
      isCorrect: false,
      showPopup: false,
      canContinue: false,
      earnedPoints: 0
    };
  },

  watch: {
    questionNum() {
      this.resetQuestion();
    }
  },

  computed: {
    currentQuestion() {
      return json.openQuestions[this.questionNum];
    },

    title() {
      return this.currentQuestion
        ? this.currentQuestion.title
        : "";
    },

    subtitle() {
      return this.currentQuestion
        ? this.currentQuestion.subtitle
        : "";
    },

    correctAnswer() {
      return this.currentQuestion
        ? this.currentQuestion.correct
        : "";
    },

    keywords() {
      return this.currentQuestion
        ? this.currentQuestion.keywords
        : [];
    }
  },

  methods: {

    normalize(text) {
      return text
        .toLowerCase()
        .replace(/[.,!?()]/g, "")
        .replace(/\s+/g, " ")
        .trim();
    },

    submitAnswer() {

      if (!this.userAnswer.trim()) return;

      const answer = this.normalize(this.userAnswer);

        let found = 0;

        this.keywords.forEach(group => {

            const keywords = Array.isArray(group)
                ? group
                : [group];

            const exists = keywords.some(keyword =>
                answer.includes(this.normalize(keyword))
            );

            if (exists) {
                found++;
            }

        });

      const ratio = found / this.keywords.length;

      this.isCorrect = ratio >= 0.75;

      this.earnedPoints = this.isCorrect ? 10 : 0;

      this.submitted = true;

      this.showPopup = true;
    },

    closePopup() {

      this.showPopup = false;

      this.canContinue = true;

    },

    handleMoveOn() {

      this.$emit("points-updated", this.earnedPoints);

      setTimeout(() => {

        this.$emit("next-question");

      }, 180);

    },

    resetQuestion() {

      this.userAnswer = "";

      this.submitted = false;

      this.isCorrect = false;

      this.showPopup = false;

      this.canContinue = false;

      this.earnedPoints = 0;

    }

  }

};
</script>
<style scoped>

#open-question {
  margin-top: 30%;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  direction: rtl;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.question-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  max-width: 400px;
  padding: 20px;
  box-sizing: border-box;
}

.question-header {
  text-align: center;
  margin-bottom: 25px;
  height: 180px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.question-title {
  color: #5c2d13;
  font-size: 28px;
  font-weight: bold;
  margin: 0 0 10px 0;
}

.question-subtitle {
  color: #6d3b1e;
  font-size: 25px;
  line-height: 1.4;
  margin: 0;
  font-weight: 500;
}

/* תיבת התשובה */

.answer-box {

  margin-top: 10%;

  width: 100%;

  min-height: 180px;

  resize: none;

  border: none;

  outline: none;

  border-radius: 20px;

  background: rgba(228,167,132,.95);

  color: white;

  font-size: 20px;

  font-family: inherit;

  padding: 18px;

  box-sizing: border-box;

  box-shadow: 0 4px 10px rgba(92,45,19,.15);

}

.answer-box::placeholder {

  color: rgba(255,255,255,.8);

}

/* כפתור הגשה */

.submit-btn {

  margin-top: 25px;

  background-color: #b97d57;

  color: white;

  font-size: 20px;

  font-weight: bold;

  border: none;

  border-radius: 16px;

  padding: 18px 30px;

  cursor: pointer;

  transition: .2s;

}

.submit-btn:hover:not(:disabled){

  transform: translateY(-2px);

}

.submit-btn:disabled{

  opacity:.5;

  cursor:not-allowed;

}

/* הודעת הצלחה / כישלון */

.result-message{

  margin-top:20px;

  font-size:22px;

  font-weight:bold;

}

.correct{

  color:#2e7d32;

}

.wrong{

  color:#c62828;

}

/* כפתור המשך */

.move-on {

  margin-top: 20px;

  background-color: #b97d57;

  color: white;

  font-size: 22px;

  font-weight: bold;

  border: none;

  border-radius: 16px;

  padding: 22px 30px;

  cursor: pointer;

  box-shadow: 0 4px 10px rgba(0,0,0,.15);

  transition: transform .18s ease,
              background-color .18s ease;

}

.move-on:active{

  transform:scale(.92);

  background:#8f5d3e;

}

/* Popup */

.popup-overlay{

  position:fixed;

  inset:0;

  background:rgba(0,0,0,.45);

  display:flex;

  justify-content:center;

  align-items:center;

  z-index:999;

}

.popup{

  width:85%;

  max-width:430px;

  background:white;

  border-radius:25px;

  padding:30px;

  text-align:center;

  position:relative;

  animation:popup .25s ease;

}

.popup h3{

  color:#5c2d13;

  margin-bottom:20px;

  font-size:28px;

}

.popup p{

  color:#444;

  font-size:22px;

  line-height:1.6;

}

.close-btn{

  position:absolute;

  top:15px;

  left:15px;

  width:38px;

  height:38px;

  border-radius:50%;

  border:none;

  background:#b97d57;

  color:white;

  font-size:22px;

  cursor:pointer;

}

.close-btn:hover{

  background:#8f5d3e;

}

@keyframes popup{

  from{

    transform:scale(.8);

    opacity:0;

  }

  to{

    transform:scale(1);

    opacity:1;

  }

}

</style>