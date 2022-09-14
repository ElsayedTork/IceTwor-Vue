<template>
  <div class="twor">
    <div class="container-fluid">
      <section class="twor__game" v-if="lose == false && stepNumber < 15">
        <div v-for="step in steps" :key="step.id">
          <div
            class="twor__game__step"
            :style="{ bottom: step.bottom + 'px', left: step.left + 'px' }"
          ></div>
        </div>
        <div class="twor__game__player">
          <img
            src="./assets/Mario.png"
            alt=""
            @keyup.right="handleRigth"
            @keyup.left="handleLeft"
            @keyup.up="handleUp"
            ref="div"
            tabindex="0"
            :class="jumb == true ? `jumb-${this.dir}` : ''"
            :style="{ bottom: this.imgx + 'px', left: this.imgy + 'px' }"
          />
        </div>
      </section>
      <section class="twor__lose" v-else>
        <h2>{{ messageEndGame }}</h2>
      </section>
      <h2 class="twor__score">Your Score Is : {{ stepNumber }}</h2>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      steps: [
        { id: 1, bottom: 100, left: 200 },
        { id: 2, bottom: 200, left: 350 },
        { id: 3, bottom: 300, left: 600 },
        { id: 4, bottom: 400, left: 500 },
        { id: 5, bottom: 500, left: 400 },
        { id: 6, bottom: 600, left: 320 },
        { id: 7, bottom: 700, left: 250 },
        { id: 8, bottom: 800, left: 150 },
        { id: 9, bottom: 900, left: 200 },
        { id: 10, bottom: 1000, left: 300 },
        { id: 11, bottom: 1100, left: 200 },
        { id: 12, bottom: 1200, left: 100 },
        { id: 13, bottom: 1300, left: 150 },
        { id: 14, bottom: 1400, left: 250 },
        { id: 15, bottom: 1500, left: 350 },
      ],
      varr: false,
      imgx: 0,
      imgy: 0,
      jumb: false,
      count: 0,
      dir: '',
      stepNumber: 0,
      lose: false,
    };
  },
  computed: {
    messageEndGame() {
      if (this.lose == true) {
        return 'You Lose The Game';
      } else if (this.stepNumber >= 15) {
        return 'You Win The Game';
      } else {
        return '';
      }
    },
  },
  methods: {
    handleRigth() {
      this.imgy += 25;
      this.movementFallsRight();
      this.varr = !this.varr;
      this.dir = 'r';
      this.jumb = true;
    },
    handleLeft() {
      this.dir = 'l';
      this.imgy -= 25;
      this.movementFallsLeft();
      this.varr = !this.varr;
      this.jumb = true;
    },
    handleUp() {
      this.imgx += 180;
      this.dir = 'up';
      this.jumb = true;
      this.varr = !this.varr;
      setTimeout(() => {
        if (
          this.imgx > this.steps[this.stepNumber].bottom &&
          this.imgy + 60 > this.steps[this.stepNumber].left &&
          this.imgy < this.steps[this.stepNumber].left + 300
        ) {
          this.imgx = this.steps[this.stepNumber].bottom + 20;
          this.stepNumber++;
        } else {
          this.imgx = 0;
        }
      }, 200);
    },
    randomNumbwr(to) {
      return Math.floor(Math.random() * to);
    },
    movementFallsRight() {
      if (this.stepNumber >= 1) {
        if (this.imgy >= this.steps[this.stepNumber - 1].left + 300) {
          this.imgx -= 100;
          this.stepNumber--;
          if (this.imgy >= this.steps[this.stepNumber - 1].left + 300) {
            this.imgx = 0;
            this.lose = true;
          }
        }
      }
    },
    movementFallsLeft() {
      if (this.stepNumber >= 1) {
        if (this.imgy + 60 < this.steps[this.stepNumber - 1].left) {
          this.imgx -= 100;
          this.stepNumber--;
          if (this.imgy + 60 < this.steps[this.stepNumber - 1].left) {
            this.imgx -= 100;
            this.stepNumber--;
            if (this.imgy + 60 < this.steps[this.stepNumber - 1].left) {
              this.imgx = 0;
              this.lose = true;
            }
          }
        }
      }
    },
  },
  mounted() {
    this.$refs.div.focus();
    setTimeout(() => {
      setTimeout(() => {
        setInterval(() => {
          this.steps = this.steps.map((step) => ({
            ...step,
            bottom: step.bottom - 5,
          }));
          this.imgx -= 5;
          if (this.imgx <= 0) {
            this.lose = true;
          }
        }, 200);
      }, 20);
    }, 2000);
  },
};
</script>

<style lang="scss" scoped>
.twor {
  &__game {
    width: 100%;
    height: 100vh;
    background-color: #000;
    position: relative;
    &__step {
      width: 300px;
      background-color: orange;
      height: 20px;
      border-radius: 10px 10px 0 0;
      border-color: #fff;
      position: absolute;
    }
    &__player {
      img {
        height: 100px;
        width: 60px;
        position: absolute;
        transition: rotate 0.7s ease-in-out;
      }
    }
    .jumb-r {
      rotate: y 50deg;
    }
    .jumb-l {
      rotate: y -50deg;
    }

    .jumb-up {
      rotate: z 360deg;
    }
  }
  &__lose {
    background-color: #000;
    color: orange;
    padding: 30px 100px;
    font-size: 45px;
    text-align: center;
  }
  &__score {
    position: absolute;
    top: 10px;
    right: 200px;
    color: #fff;
    font-size: 20px;
  }
}
</style>
