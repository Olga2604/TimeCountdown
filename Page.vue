<template>
  <div>
    <h1>Time Countdown</h1>
    <time-countdown :date="1551366000" /> //date must be send as a Unix timestamp
  </div>
</template>

<script>
import TimeCountdown from '../elements/TimeCountdown';

export default {
  components: { TimeCountdown },
};
</script>
