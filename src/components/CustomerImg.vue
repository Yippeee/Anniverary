.<template>
  <div class="card" @click="cardClickFn()">
    <div class="card__face card__face--front">
      <!-- <img src="../assets/2018-12.jpeg" /> -->
      <span>{{ brief }}</span>
    </div>
    <div class="card__face card__face--back">
      <img :src="src" />
    </div>
  </div>
</template>

<script>
export default {
  props: {
      brief: String,
      src: String,
  },
  setup(props, { emit }) {
    return {
      cardClickFn: () => emit("clickFn"),
    };
  },
};
</script>

<style lang="less">
.card {
  position: relative;
  height: 173px;
  width: 109px;
  color: white;
  cursor: pointer;
  transition: 1s ease-in-out;
  transform-style: preserve-3d;

  &:hover {
    transform: rotateY(0.5turn);
  }
  .card__face--front {
    line-height: 173px;
  }

  .card__face {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    transition: 1s ease-in-out;
    // -webkit-box-reflect: below 0 linear-gradient(transparent, transparent, rgba(0, 0, 0, 0.4));

    img {
      height: 100%;
      width: 100%;
      object-fit: contain;
    }

    &--back {
      transform: rotateY(0.5turn);
    }
  }
}
</style>
