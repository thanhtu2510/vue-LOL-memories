<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <interact-screen
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult"
  />
  <result-screen
    v-if="statusMatch === 'result'"
    :timer="timer"
    @onStartAgain="statusMatch = 'default'"
  />
  <copy-right />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import CopyRight from "./components/CopyRight";

import { shuffled } from "./utils/array";
export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRight,
  },
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [], //mảng chứa card đã mix
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
    };
  },
  methods: {
    onHandleBeforeStart(config) {
      console.log("running...", config);
      this.settings.totalOfBlocks = config.totalOfBlocks;
      //tạo ra 2 mảng có 1 nửa số card so với chế độ để mix
      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      //mảng thứ 2 giống mảng 1
      const secondCards = [...firstCards];
      //mix 2 mảng lại
      const cards = [...firstCards, ...secondCards];

      console.log(cards);
      //dùng hàm shuffled đã định nghĩa ở file array.js để mix
      this.settings.cardsContext = shuffled(
        shuffled(shuffled(shuffled(cards)))
      );
      console.log(this.settings.cardsContext);
      //sau khi đã có dữ liệu thì sẽ tính thời gian bắt đầu chơi
      this.settings.startedAt = new Date().getTime();

      //data ready
      this.statusMatch = "match";
    },
    onGetResult() {
      //get timer
      this.timer = new Date().getTime() - this.settings.startedAt;
      //switch to result component
      this.statusMatch = "result";
    },
  },
};
</script>
