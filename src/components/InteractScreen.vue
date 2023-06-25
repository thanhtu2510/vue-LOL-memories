<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 +
            16) *
          Math.sqrt(cardsContext.length)
        }px`,
      }"
    >
      <card-flip
        v-for="(card, index) in cardsContext"
        :key="index"
        :ref="`card-${index}`"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{ index: index, value: card }"
        :rules="rules"
        :cardsContext="cardsContext"
        @onFlip="checkRule($event)"
      />
    </div>
  </div>
</template>

<script>
import CardFlip from "./Card.vue";
export default {
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  components: {
    CardFlip,
  },
  data() {
    return {
      rules: [],
    };
  },
  methods: {
    checkRule(card) {
      if (this.rules.length === 2) return false;

      this.rules.push(card);

      // if (this.rules[0].index != this.rules[1].index) {
      //   if (
      //     this.rules.length === 2 &&
      //     this.rules[0].value === this.rules[1].value
      //   ) {
      //     console.log("Right...");
      //     this.$refs[`card-${this.rules[0].index}`][0].onEnableDisableMode();
      //     this.$refs[`card-${this.rules[1].index}`][0].onEnableDisableMode();
      //     //reset rules
      //     this.rules = [];
      //     const disabledElements = document.querySelectorAll(
      //       ".screen .card.disabled"
      //     );
      //     console.log(disabledElements);
      //     if (
      //       disabledElements &&
      //       disabledElements.length === this.cardsContext.length - 2
      //     ) {
      //       console.log("Finished");
      //       setTimeout(() => {
      //         this.$emit("onFinish");
      //       }, 920);
      //     }
      //   } else if (
      //     this.rules.length === 2 &&
      //     this.rules[0].value !== this.rules[1].value
      //   ) {
      //     console.log("Wrong...");
      //     setTimeout(() => {
      //       //close two cards
      //       this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
      //       this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
      //       this.rules = [];
      //     }, 800);

      //     //reset lại rules []
      //   } else return false;
      // } else this.rules = [];

      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        console.log("Right...");
        this.$refs[`card-${this.rules[0].index}`][0].onEnableDisableMode();
        this.$refs[`card-${this.rules[1].index}`][0].onEnableDisableMode();
        //reset rules
        this.rules = [];
        const disabledElements = document.querySelectorAll(
          ".screen .card.disabled"
        );
        console.log(disabledElements);
        if (
          disabledElements &&
          disabledElements.length === this.cardsContext.length - 2
        ) {
          console.log("Finished");
          setTimeout(() => {
            this.$emit("onFinish");
          }, 920);
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        console.log("Wrong...");
        setTimeout(() => {
          //close two cards
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
          this.rules = [];
        }, 800);

        //reset lại rules []
      } else return false;
    },
  },
};
</script>

<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: rgb(35, 39, 44);
  color: var(--light);
}

.screen__inner {
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
