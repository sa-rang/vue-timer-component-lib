<template>
  <div class="burst-timer">
    <div class="base-timer">
      <svg class="base-timer__svg" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
        <g class="base-timer__circle">
          <circle class="base-timer__path-elapsed" cx="50" cy="50" r="45"></circle>
          <path :stroke-dasharray="dashPathRemaining" :class="remainingPathColor" class="base-timer__path-remaining" d="
          M 50, 50
          m -45, 0
          a 45,45 0 1,0 90,0
          a 45,45 0 1,0 -90,0
        "></path>
        </g>
      </svg>
      <span id="base-timer-label" class="base-timer__label">{{
        timeLeftStr
      }}</span>
    </div>
  </div>
</template>


<script setup lang="ts">
import { ref, onMounted, onUnmounted } from "vue";
interface Props {
  time: number
}

const props = defineProps<Props>()
const emit = defineEmits(['timesup'])
const FULL_DASH_ARRAY = 283;
const WARNING_THRESHOLD = 10;
const ALERT_THRESHOLD = 5;

const COLOR_CODES = {
  info: {
    color: "green",
  },
  warning: {
    color: "orange",
    threshold: WARNING_THRESHOLD,
  },
  alert: {
    color: "red",
    threshold: ALERT_THRESHOLD,
  },
};

const TIME_LIMIT = props.time; //scy3 300
let timePassed = 0;
const timeLeft = ref(TIME_LIMIT);
const timeLeftStr = ref("");
let timerInterval = <any>null;
const remainingPathColor = ref(COLOR_CODES.info.color);
const dashPathRemaining = ref(FULL_DASH_ARRAY.toString());

onMounted(async () => {
  startTimer();
});

onUnmounted(() => {
  clearInterval(timerInterval);
});

const onTimesUp = () => {
  clearInterval(timerInterval);
  emit("timesup", true);
};

const startTimer = () => {
  timerInterval = setInterval(() => {
    timePassed = timePassed += 1;
    timeLeft.value = TIME_LIMIT - timePassed;
    timeLeftStr.value = formatTime(timeLeft.value);
    setCircleDasharray();
    setRemainingPathColor(timeLeft.value);

    if (timeLeft.value === 0) {
      onTimesUp();
    }
  }, 1000);
};

const formatTime = (time: number) => {
  const minutes = Math.floor(time / 60);
  let seconds = time % 60;
  let secondsStr = "";

  if (seconds < 10) {
    secondsStr = `0${seconds}`;
  } else {
    secondsStr = `${seconds}`;
  }

  return `${minutes}:${secondsStr}`;
};

const setRemainingPathColor = (timeLeft: number) => {
  const { alert, warning, info } = COLOR_CODES;
  if (timeLeft <= alert.threshold) {
    remainingPathColor.value = alert.color;
  } else if (timeLeft <= warning.threshold) {
    remainingPathColor.value = warning.color;
  }
};

const calculateTimeFraction = () => {
  const rawTimeFraction = timeLeft.value / TIME_LIMIT;
  return rawTimeFraction - (1 / TIME_LIMIT) * (1 - rawTimeFraction);
};

const setCircleDasharray = () => {
  const circleDasharray = `${(
    calculateTimeFraction() * FULL_DASH_ARRAY
  ).toFixed(0)} 283`;
  dashPathRemaining.value = circleDasharray;
};


</script>

<style lang="scss" scoped>
.burst-timer {
  display: flex;
  justify-content: center;
  margin-top: 20px;

  .base-timer {
    position: relative;
    width: 120px;
    height: 120px;
  }

  .base-timer__svg {
    transform: scaleX(-1);
  }

  .base-timer__circle {
    fill: none;
    stroke: none;
  }

  .base-timer__path-elapsed {
    stroke-width: 7px;
    stroke: grey;
  }

  .base-timer__path-remaining {
    stroke-width: 7px;
    stroke-linecap: round;
    transform: rotate(90deg);
    transform-origin: center;
    transition: 1s linear all;
    fill-rule: nonzero;
    stroke: currentColor;
  }

  .base-timer__path-remaining.green {
    color: rgb(65, 184, 131);
  }

  .base-timer__path-remaining.orange {
    color: orange;
  }

  .base-timer__path-remaining.red {
    color: red;
  }

  .base-timer__label {
    position: absolute;
    width: 120px;
    height: 120px;
    top: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 24px;
  }
}
</style>
