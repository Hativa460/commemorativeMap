<template>
  <div id="test">

    <transition name="fade" mode="out-in">
      <open-question v-if="question === 1" :key="1" :questionNum="0" @points-updated="addPoints" @next-question="goToNextQuestion" />
      <sequence-operations v-if="question === 2" :key="1" :questionNum="0" @points-updated="addPoints" @next-question="goToNextQuestion" />
      <american-question v-if="question === 3" :key="1" :questionNum="0" @points-updated="addPoints" @next-question="goToNextQuestion" />
      <select-multiple v-if="question === 4" :key="1" :questionNum="0" @points-updated="addPoints" @next-question="goToNextQuestion" />
      <american-question v-if="question === 5" :key="2" :questionNum="1" @points-updated="addPoints" @next-question="goToNextQuestion" />
      <sentence-completion v-if="question === 6" :key="1" :questionNum="0" @points-updated="addPoints" @next-question="goToNextQuestion" />
      <open-question v-if="question === 7" :key="2" :questionNum="1" @points-updated="addPoints" @next-question="goToNextQuestion" />
      <sortable-question v-if="question === 8" :key="1" :questionNum="0" @points-updated="addPoints" @next-question="toFinal" />


    </transition>

  </div>
</template>

<script>
import AmericanQuestion from "./AmericanQuestion.vue";
import OpenQuestion from "./OpenQuestion.vue";
import SortableQuestion from "./SortableQuestion.vue";
import SelectMultiple from "./SelectMultiple.vue";
import SequenceOperations from "./SequenceOperations.vue";
import SentenceCompletion from "./SentenceCompletion.vue";
export default {
  name: "test-page",

  components: {
    AmericanQuestion,
    OpenQuestion,
    SortableQuestion,
    SelectMultiple,
    SequenceOperations,
    SentenceCompletion
  },

  data() {
    return {
      question: 1,
      totalPoints: 0
    };
  },

  methods: {
    goToNextQuestion() {
      this.question++;
      console.log(this.question);
    },

    addPoints(points) {
      this.totalPoints += points;
      console.log("הניקוד המעודכן באב:", this.totalPoints);
    },
    toFinal() {
      this.$emit("to-end",  this.totalPoints);
      console.log("הניקוד המעודכן באב:", this.totalPoints);
    },
  }
};
</script>

<style>
#test {
  margin: 0%;
  overflow-x: hidden;
  height: 100%;
  width: 100vw;
  position: absolute;
  top: 0;
  right: 0;
  background-image: url("~@/assets/media/questionbg.png");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity .25s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>