<template>
  <div id="american">
    <div class="question-container">

      <div class="question-header">
        <h2 class="question-title">{{ title }}</h2>
        <p class="question-subtitle">{{ subtitle }}</p>
      </div>

      <div class="answers-grid">
        <button
          v-for="(answer, index) in answers"
          :key="index"
          class="answer-card"
          @click="selectAns(index)"
          :class="{
            'correct-ans': isClicked && index === selectedAns && index === correctAns,
            'wrong-ans': isClicked && index === selectedAns && index !== correctAns
          }"
          :disabled="isClicked"
        >
          {{ answer }}
        </button>
      </div>

      <button
        class="move-on"
        :class="{ pressed: moving }"
        @click="handleMoveOn"
      >
        המשך
      </button>

    </div>
  </div>
</template>

<script>
import json from "../../text.json";

export default {
  name: "american-question",

  props: ["questionNum"],

  data() {
    return {
      selectedAns: -1,
      isClicked: false,
      moving: false
    };
  },

  watch: {
    questionNum() {
      this.selectedAns = -1;
      this.isClicked = false;
      this.moving = false;
    }
  },

  computed: {
    title() {
      return json.american[this.questionNum]
        ? json.american[this.questionNum].title
        : "";
    },

    subtitle() {
      return json.american[this.questionNum]
        ? json.american[this.questionNum].subtitle
        : "";
    },

    answers() {
      return json.american[this.questionNum]
        ? json.american[this.questionNum].answers
        : [];
    },

    correctAns() {
      return json.american[this.questionNum]
        ? json.american[this.questionNum].correct
        : -1;
    }
  },

  methods: {
    selectAns(index) {
      if (this.isClicked) return;

      this.selectedAns = index;
      this.isClicked = true;
    },

    handleMoveOn() {
      if (this.selectedAns === -1) {
        alert("נא לבחור תשובה לפני שממשיכים!");
        return;
      }

      this.moving = true;

      const points =
        this.selectedAns === this.correctAns ? 7.5 : 0;

      this.$emit("points-updated", points);

      setTimeout(() => {
        this.$emit("next-question");
      }, 180);
    }
  }
};
</script>

<style scoped>
#american {
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

.answers-grid {
  margin-top: 10%;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 15px;
  width: 100%;
  margin-bottom: 30px;
}

.answer-card {
  background-color: rgba(228, 167, 132, 0.95);
  color: #ffffff;
  font-size: 16px;
  font-weight: bold;
  border: 2px solid transparent;
  border-radius: 20px;
  min-height: 95px;
  height: 100px;
  padding: 10px;
  cursor: pointer;
  box-shadow: 0 4px 10px rgba(92, 45, 19, 0.15);
  transition: transform .2s ease,
              box-shadow .2s ease,
              background-color .2s ease;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.answer-card:hover:not(:disabled) {
  transform: translateY(-3px);
  box-shadow: 0 6px 15px rgba(92, 45, 19, 0.25);
}

.answer-card:disabled {
  cursor: not-allowed;
}

.answer-card.correct-ans {
  background-color: rgb(179, 241, 160) !important;
  color: black !important;
  border-color: #1b5e20 !important;
}

.answer-card.wrong-ans {
  background-color: rgb(247, 83, 83) !important;
  color: white !important;
  border-color: #b71c1c !important;
}

.move-on {
  background-color: #b97d57;
  color: white;
  font-size: 22px;
  font-weight: bold;
  border: none;
  border-radius: 16px;
  padding: 22px 30px;
  cursor: pointer;

  transition:
    transform .18s ease,
    background-color .18s ease,
    box-shadow .18s ease;

  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.15);

  -webkit-tap-highlight-color: transparent;
}

.move-on.pressed {
  transform: scale(.92);
  background-color: #8f5d3e;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.2);
}
</style>