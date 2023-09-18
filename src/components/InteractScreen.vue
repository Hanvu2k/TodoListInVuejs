<template>
  <div class="screen">
    <div
      class="screen-inner"
      :style="{
        width: `${
          ((((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) / 4 +
            16) *
          Math.sqrt(cardContext.length)
        }px`,
      }"
    >
      <card-flip
        v-for="(card, index) in cardContext"
        :key="index"
        :imgBackFaceUrl="`pokeimg/${card}.png`"
        :card="{ index, value: card }"
        :ref="`card-${index}`"
        :cardContext="cardContext"
        @onFlip="checkRules($event)"
      />
    </div>
  </div>
</template>

<script>
import CardFlip from "./CardFlip.vue";
export default {
  props: {
    cardContext: {
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
    checkRules(card) {
      if (this.rules.length === 2) return false;
      this.rules.push(card);

      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        //add class 'disabled'
        this.$refs[`card-${this.rules[0].index}`][0].onEnabledDisableMode();
        this.$refs[`card-${this.rules[1].index}`][0].onEnabledDisableMode();

        // reset rules
        this.rules = [];

        const disabledElements = document.querySelectorAll(
          ".screen .card-wrapper.disabled"
        );
        if (
          disabledElements &&
          disabledElements.length === this.cardContext.length - 2
        ) {
          setTimeout(() => {
            this.$emit("onFinished");
          }, 900);
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        setTimeout(() => {
          // close two card
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
          // reset the rules to []
          this.rules = [];
        }, 800);
      } else return false;
    },
  },
};
</script>

<style lang="scss" scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);

  .screen-inner {
    display: flex;
    flex-wrap: wrap;
    margin: 2rem auto;
  }
}
</style>
