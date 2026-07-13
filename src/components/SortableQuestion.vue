<template>
  <div id="sortable">

    <div class="question-container">

      <div class="question-header">

        <h2 class="question-title">
          {{ title }}
        </h2>

        <p class="question-subtitle">
          {{ subtitle }}
        </p>

      </div>

      <div class="steps-container">

       <div
        v-for="(item,index) in items"
        :key="item"
        class="step"
        :class="stepClass(index)"
        @click="selectStep(index)"
        >

          {{ item }}

        </div>

      </div>

      <button
        class="submit-btn"
        @click="submit"
        v-if="!submitted"
      >
        הגש
      </button>

      <button
        class="move-on"
        v-if="submitted"
        @click="nextQuestion"
      >
        המשך
      </button>

    </div>

  </div>
</template>
<script>
import json from "../../text.json";

export default {
  name: "SortableQuestion",
  props: ["questionNum"],

data() {
  return {
    items: [],
    selectedIndex: null,
    submitted: false,
    points: 0
  };
},

  computed: {
    question() {
      return json.sortable[this.questionNum];
    },

    title() {
      return this.question ? this.question.title : "";
    },

    subtitle() {
      return this.question ? this.question.subtitle : "";
    },

    correctList() {
      return this.question ? this.question.correctList : [];
    }
  },

  watch: {
    questionNum() {
      this.loadQuestion();
    }
  },
    mounted() {
    this.loadQuestion();
    },

methods: {
  loadQuestion() {
    this.items = [...this.correctList];
    this.shuffle();

    this.selectedIndex = null;
    this.submitted = false;
    this.points = 0;
  },

  shuffle() {
    do {
      for (let i = this.items.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [this.items[i], this.items[j]] = [this.items[j], this.items[i]];
      }
    } while (
      this.items.every((item, index) => item === this.correctList[index])
    );
  },

  selectStep(index) {
    if (this.submitted) return;

    if (this.selectedIndex === null) {
      this.selectedIndex = index;
      return;
    }

    const temp = this.items[this.selectedIndex];

    this.$set(this.items, this.selectedIndex, this.items[index]);
    this.$set(this.items, index, temp);

    this.selectedIndex = null;
  },

  submit() {
    this.submitted = true;

    let correct = 0;

    this.items.forEach((item, index) => {
      if (item === this.correctList[index]) {
        correct++;
      }
    });

    this.points = correct*5;
      console.log(correct);
  },

  stepClass(index) {
    if (!this.submitted) {
      return this.selectedIndex === index
        ? "selected"
        : "";
    }

    return this.items[index] === this.correctList[index]
      ? "correct"
      : "wrong";
  },

  nextQuestion() {
    this.$emit("points-updated", this.points);

    setTimeout(() => {
      this.$emit("next-question");
    }, 180);
  }
}
};
</script>
<style scoped>

#sortable {
  margin-top: 30%;
  width: 100%;
  display: flex;
  justify-content: center;
  direction: rtl;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.question-container {
  width: 100%;
  max-width: 400px;
  padding: 20px;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.question-header {
  text-align: center;
  height: 170px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.question-title {
  color: #5c2d13;
  font-size: 28px;
  font-weight: bold;
  margin: 0 0 10px;
}

.question-subtitle {
  color: #6d3b1e;
  font-size: 24px;
  line-height: 1.4;
  margin: 0;
}

.steps-container {
  width: 100%;
  margin-top: 20px;
}

.step {
  background: rgba(228,167,132,.95);
  color: white;
  border: 3px dashed #5c2d13;
  border-radius: 16px;
  padding: 18px;
  margin-bottom: 12px;
  font-size: 18px;
  font-weight: bold;
  text-align: center;
  cursor: grab;
  user-select: none;
  transition: .25s;
  box-shadow: 0 4px 10px rgba(92,45,19,.18);
}

.step:hover {
  transform: translateY(-2px);
  box-shadow: 0 7px 18px rgba(92,45,19,.25);
}

.step:active {
  cursor: grabbing;
  transform: scale(.97);
}

.correct {
  border: 4px solid #2e7d32 !important;
  background: rgba(228,167,132,.95);
}

.wrong {
  border: 4px solid #c62828 !important;
  background: rgba(228,167,132,.95);
}

.submit-btn,
.move-on {
  margin-top: 25px;
  background: #b97d57;
  color: white;
  border: none;
  border-radius: 16px;
  padding: 20px 35px;
  font-size: 22px;
  font-weight: bold;
  cursor: pointer;
  transition: .2s;
  box-shadow: 0 4px 10px rgba(0,0,0,.18);
}

.submit-btn:hover,
.move-on:hover {
  transform: translateY(-2px);
}

.submit-btn:active,
.move-on:active {
  transform: scale(.92);
  background: #8f5d3e;
}

.submit-btn,
.move-on {
  -webkit-tap-highlight-color: transparent;
}
.selected{
  border:4px solid #0d47a1 !important;
  box-shadow:0 0 18px rgba(13,71,161,.35);
}
</style>
