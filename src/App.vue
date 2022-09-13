<template>
  <div class="container">
    <section class="twor" v-if="lose == false">
      <div v-for="step in steps" :key="step.id">
        <div
          class="twor__step"
          :style="{ bottom: step.bottom + 'px', left: step.left + 'px' }"
        ></div>
      </div>
      <div class="twor__player">
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
    <section class="tower-lose" v-else>
      <h2>You Lose</h2>
    </section>
    <h2 class="score">Your Score Is : {{ stepNumber }}</h2>
  </div>
</template>

<script>
export default {
  data() {
    return {
      steps: [],
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
      // if (this.stepNumber % 2 == 0 && this.stepNumber != 0) {
      //   console.log('%3');
      //   this.steps = this.steps.map((step) => [
      //     ...step,
      //   ]);
      // }

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
          if (this.imgy >= this.steps[this.stepNumber - 2].left + 300) {
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
          if (this.imgy + 60 < this.steps[this.stepNumber - 2].left) {
            this.imgx = 0;
            this.lose = true;
          }
        }
      }
    },
  },
  created() {
    for (let x = 0; x <= 20; x++) {
      this.steps.push({
        id: this.randomNumbwr(1000),
        bottom: this.count,
        left: this.randomNumbwr(500),
      });
    }
    this.steps = [
      { id: 1, bottom: 100, left: 200 },
      { id: 2, bottom: 200, left: 350 },
      { id: 3, bottom: 300, left: 600 },
      { id: 4, bottom: 400, left: 500 },
      { id: 5, bottom: 500, left: 400 },
      { id: 6, bottom: 600, left: 220 },
      { id: 7, bottom: 700, left: 150 },
      { id: 8, bottom: 800, left: 600 },
      { id: 9, bottom: 900, left: 870 },
      { id: 10, bottom: 1000, left: 400 },
    ];
  },
  mounted() {
    this.$refs.div.focus();
    if (this.stepNumber > 0) {
      setTimeout(() => {
        setInterval(() => {
          this.steps = this.steps.map((step) => ({
            ...step,
            bottom: step.bottom - 5,
          }));
        }, 200);
      }, 20);
    }
  },
};
</script>

<style lang="scss" class="scoped">
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

html {
  font-family: 'Roboto', sans-serif;
}
.twor {
  width: 100%;
  height: 95vh;
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
.tower-lose {
  background-color: #000;
  color: orange;
  padding: 30px 100px;
  font-size: 45px;
  text-align: center;
}
.score {
  position: absolute;
  top: 10px;
  right: 200px;
  color: #fff;
  font-size: 20px;
}
</style>
