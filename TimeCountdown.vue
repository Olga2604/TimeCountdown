<template>
  <div class="time-countdown">
    <div class="time-countdown__block">
      <div class="time-countdown__block__digit">
        <div v-html="formatDigits(getHours)">
        </div>
      </div>
      <div class="time-countdown__block__divider">
        <div></div>
        <div></div>
      </div>
      <div class="time-countdown__block__text">Hours</div>
    </div>
    <div class="time-countdown__block">
      <div class="time-countdown__block__digit">
        <div v-html="formatDigits(getMinutes)">
        </div>
      </div>
      <div class="time-countdown__block__divider">
        <div></div>
        <div></div>
      </div>
      <div class="time-countdown__block__text">Minutes</div>
    </div>
    <div class="time-countdown__block">
      <div class="time-countdown__block__digit">
        <div v-html="formatDigits(getSeconds)">
        </div>
      </div>
      <div class="time-countdown__block__text">Seconds</div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['date'],
  data() {
    return {
      countdownDate: Math.trunc(this.date),
      currentDate: Math.trunc(new Date().getTime() / 1000),
    };
  },
  created() {
    this.updateCountdown();
  },
  computed: {
    getHours() {
      return Math.trunc((this.countdownDate - this.currentDate) / 60 / 60).toString();
    },
    getMinutes() {
      return (Math.trunc((this.countdownDate - this.currentDate) / 60) % 60).toString();
    },
    getSeconds() {
      return ((this.countdownDate - this.currentDate) % 60).toString();
    },
  },
  methods: {
    updateCountdown() {
      this.currentDate = setInterval(() => {
        this.currentDate = Math.trunc((new Date()).getTime() / 1000);
      }, 1000);
    },
    formatDigits(digit) {
      const newFormattedDigit = this.textFormattedDigit(digit);
      return (newFormattedDigit.length > 1)
        ? `${newFormattedDigit
          .split('')
          .map(el => `<div class="digit-block"><div class="flipping-block">${el}</div></div>`)
          .join('')}`
        : `<div class="digit-block"><div class="flipping-block"${newFormattedDigit}</div></div`;
    },
    textFormattedDigit(digitValue) {
      let formattedDigit;
      if (digitValue.length <= 1) {
        formattedDigit = `0${digitValue}`;
        return formattedDigit;
      } return digitValue;
    },
  },
};
</script>

<style lang="scss" scoped>
  .time-countdown {
    &__block {
      display: inline-block;
      margin: 5px;
      &__divider {
        display: inline-block;
        padding-left: 10px;
        & div {
          width: 5px;
          height: 5px;
          background: #ffffff;
          margin-bottom: 5px;
          border-radius: 50%;
        }
      }
      &__digit {
        display: inline-block;
        /deep/ .digit-block {
          background: linear-gradient(to bottom, #212123 50%, #262549 50%);
          display: inline-block;
          border-radius: 4px;
          margin: 2px;
        }
        /deep/ .flipping-block {
          display: inline-block;
          width: 35px;
          text-align: center;
          background: #262549;
          font-size: 40px;
          margin: 2px;
          -webkit-animation: flip 1s;
          -moz-animation: flip 1s;
          animation: flip 1s;
          border-radius: 4px;
          position: relative;
          z-index: 2;
        }
      }
      &__text {
        font-size: 12px;
        text-transform: capitalize;
        color: #8f9ecb;
      }
      &__text:nth-child(n+3) {
        margin-right: 10px;
      }
    }
  }
</style>
