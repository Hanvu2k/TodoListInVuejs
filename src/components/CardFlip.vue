<template>
  <div
    class="card-wrapper"
    :class="{ disabled: isDisabled }"
    :style="{
      height: `${(920 - 64) / Math.sqrt(cardContext.length) - 16}px`,
      width: `${(((920 - 64) / Math.sqrt(cardContext.length) - 16) * 3) / 4}px`,
      perspective: `${
        ((((920 - 64) / Math.sqrt(cardContext.length) - 16) * 3) / 4) * 2
      }px`,
    }"
  >
    <div
      class="card-inner"
      :class="{ 'is-filpped': isFlipped }"
      @click="onToggleFlipCard"
    >
      <div class="card-face card-face-front">
        <div
          class="card-content"
          :style="{
            'background-size': `${
              (((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) /
              4 /
              3
            }px ${
              (((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) /
              4 /
              3
            }px`,
          }"
        ></div>
      </div>
      <div class="card-face card-face-back">
        <div
          class="card-content"
          :style="{
            backgroundImage: `url(${require('@/assets/' + imgBackFaceUrl)})`,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    card: {
      type: [String, Number, Array, Object],
    },
    imgBackFaceUrl: {
      type: String,
      required: true,
    },
    cardContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  data() {
    return {
      isFlipped: false,
      isDisabled: false,
    };
  },
  methods: {
    onToggleFlipCard() {
      if (this.isDisabled) return;
      this.isFlipped = !this.isFlipped;
      if (this.isFlipped) this.$emit("onFlip", this.card);
    },

    onFlipBackCard() {
      this.isFlipped = false;
    },

    onEnabledDisableMode() {
      this.isDisabled = true;
    },
  },
};
</script>

<style lang="scss" scoped>
.card-wrapper {
  display: block;
  margin: 0 1rem 1rem 0;
  height: 120px;
  width: 90px;

  &.disabled {
    cursor: default;
  }

  .card-inner {
    width: 100%;
    height: 100%;
    transition: transform 1s;
    transform-style: preserve-3d;
    cursor: pointer;
    position: relative;

    &.is-filpped {
      transform: rotateY(-180deg);
    }

    .card-face {
      position: absolute;
      width: 100%;
      height: 100%;
      backface-visibility: hidden;
      overflow: hidden;
      border-radius: 1rem;
      padding: 1rem;
      box-shadow: 0 3px 10px 3px rgba($color: #000000, $alpha: 0.2);
    }

    .card-face-front {
      .card-content {
        background: url(../assets/pokeimg/icon_back.png) no-repeat center center;
        height: 100%;
        width: 100%;
      }
    }

    .card-face-back {
      background-color: var(--light);
      transform: rotateY(-180deg);

      .card-content {
        height: 100%;
        width: 100%;
        background-size: contain;
        background-repeat: no-repeat;
        background-position: center center;
      }
    }
  }
}
</style>
