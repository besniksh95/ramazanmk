<template>
  <div id="timer">
    <span id="hours">{{ hours }}</span>
    <span id="middle">:</span>
    <span id="minutes">{{ minutes }}</span>
    <span id="middle">:</span>
    <span id="seconds">{{ seconds }}</span>
  </div>
</template>

<script>
import Vue from 'vue';
import VueConfetti from "vue-confetti";

Vue.use(VueConfetti);

export default {
  props: ["start"],
  data() {
    return {
      timer: null,
      totalTime: null,
      resetButton: true,
      current: this.start
    };
  },
  methods: {
    startTimer() {
      let moment = this.$dayjs();
      let remaining = this.$dayjs(this.current).diff(
        moment,
        "millisecond",
        true
      );

      this.totalTime = remaining / 1000;
      this.timer = setInterval(() => this.countdown(), 1000);
    },
    stopTimer() {
      clearInterval(this.timer);
      this.timer = null;
      this.resetButton = true;
    },
    resetTimer() {
      this.totalTime = 0;
      clearInterval(this.timer);
      this.timer = null;
      this.resetButton = false;
    },
    padTime(time) {
      return (time < 10 ? "0" : "") + time;
    },
    countdown() {
      this.totalTime--;
    }
  },
  computed: {
    hours() {
      return this.padTime(Math.floor(this.totalTime / 3600));
    },
    minutes() {
      return this.padTime(
        Math.floor((this.totalTime - this.hours * 3600) / 60)
      );
    },
    seconds() {
      return this.padTime(
        Math.floor(this.totalTime - this.hours * 3600 - this.minutes * 60)
      );
    }
  },
  created() {
    this.startTimer();
  },
  watch: {
    start() {
      this.current = this.start;
      this.resetTimer();
      this.startTimer();
    },
    totalTime(value) {
      if (value < 1) {
        this.resetTimer();
        this.$confetti.start();

        setTimeout(() => {
          this.$confetti.stop();
          location.reload();
        }, 10000);

      }
    }
  }
};
</script>
