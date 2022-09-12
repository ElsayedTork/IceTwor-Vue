<template>
  <div class="container">
    <section class="twor">
      <div v-for="step in steps" :key="step.id">
        <div
          class="twor__step"
          :style="{ top: step.top + 'px', left: step.left + 'px' }"
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
          :style="{ bottom: this.imgx + '%', left: this.imgy + '%' }"
        />
      </div>
    </section>
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
    };
  },
  methods: {
    handleRigth() {
      this.imgy += 2;
      this.varr = !this.varr;
      this.dir = 'r';
      this.jumb = true;
    },
    handleLeft() {
      this.dir = 'l';
      this.imgy -= 2;
      this.varr = !this.varr;
      this.jumb = true;
    },
    handleUp() {
      this.imgx += 25;
      this.dir = 'up';
      this.jumb = true;
      this.varr = !this.varr;
      setTimeout(() => (this.imgx = 0), 500);
    },
    randomNumbwr(to) {
      return Math.floor(Math.random() * to);
    },
  },
  created() {
    for (let x = 0; x <= 20; x++) {
      this.count -= 220;
      this.steps.push({
        id: this.randomNumbwr(1000),
        top: this.count,
        left: this.randomNumbwr(500),
      });
    }
    this.steps = [
      { id: 1, top: 50, left: 220 },
      { id: 2, top: 150, left: 150 },
      { id: 3, top: 250, left: 600 },
      { id: 4, top: 350, left: 870 },
      { id: 5, top: 450, left: 400 },
      { id: 6, top: -50, left: 220 },
      { id: 7, top: -150, left: 150 },
      { id: 8, top: -250, left: 600 },
      { id: 9, top: -350, left: 870 },
      { id: 10, top: -450, left: 400 },
    ];
  },
  mounted() {
    this.$refs.div.focus();
    setTimeout(() => {
      setInterval(() => {
        this.steps = this.steps.map((step) => ({
          ...step,
          top: step.top + 5,
        }));
      }, 200);
    }, 20);
  },
  components: {},
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
    width: 350px;
    background-color: orange;
    height: 20px;
    border-radius: 10px 10px 0 0;
    border-color: #fff;
    position: absolute;
  }
  &__player {
    img {
      height: 100px;
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
</style>
