<template>
  <div
    class="card"
    :class="{ disabled: isDisabled }"
    :style="{
      height: `${(920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16}px`,
      width: `${(((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4}px`,
      perspective: `${((((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4) * 2}px`,
    }"
  >
    <div class="card__inner" :class="{ 'is-flipped': isFlipped }" @click="onToggleFlipCard">
      <div class="card__face card__face--front">
        <div
          class="card__content"
          :style="{
            'background-size': `${
              (((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 / 3
            }px ${(((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 / 3}px`,
          }"
        ></div>
      </div>
      <div class="card__face card__face--back">
        <div
          class="card__content"
          :style="{ backgroundImage: `url('/assets/${imgBackFaceUrl}')` }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    card: {
      type: [Array, String, Number, Object],
    },
    imgBackFaceUrl: {
      type: String,
      required: true,
    },
    cardsContext: {
      type: Array,
      default: function () {
        return []
      },
    },
  },
  data() {
    return {
      isFlipped: false,
      isDisabled: false,
    }
  },
  mounted() {
    // console.log('imgBackFaceUrl:', this.imgBackFaceUrl)
  },
  methods: {
    onToggleFlipCard() {
      if (this.isDisabled) return false
      this.isFlipped = !this.isFlipped
      if (this.isFlipped) this.$emit('onFlip', this.card)
    },
    onFlipBackCard() {
      this.isFlipped = false
    },
    onEnabledDisableMode() {
      this.isDisabled = true
    },
  },
}
</script>


<style lang="css" scoped>
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
}

.card__inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}

.card__inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
}

.card__inner.is-flipped {
  transform: rotateY(-180deg);
}

.card__face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}

.card__face--front .card__content {
  background: url('/assets/images/icon_back.png') no-repeat center center;
  width: 100%;
  height: 100%;
}

.card__face--back {
  background-color: var(--light);
  transform: rotateY(-180deg);
}

.card__face--back .card__content {
  background-size: contain;
  background-position: center center;
  background-repeat: no-repeat;
  height: 100%;
  width: 100%;
}
</style> 

