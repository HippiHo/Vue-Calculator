<template>
  <div class="calculator">
    <div class="display">{{ current || 0 }}</div>
    <div class="btn adjust" @click="clear">C</div>
    <div class="btn adjust" @click="sign">+/-</div>
    <div class="btn adjust" @click="percent">%</div>
    <div class="btn operator" @click="divide">÷</div>
    <div class="btn" @click="append('7')">7</div>
    <div class="btn" @click="append('8')">8</div>
    <div class="btn" @click="append('9')">9</div>
    <div class="btn operator" @click="times">x</div>
    <div class="btn" @click="append('4')">4</div>
    <div class="btn" @click="append('5')">5</div>
    <div class="btn" @click="append('6')">6</div>
    <div class="btn operator" @click="minus">−</div>
    <div class="btn" @click="append('1')">1</div>
    <div class="btn" @click="append('2')">2</div>
    <div class="btn" @click="append('3')">3</div>
    <div class="btn operator" @click="add">+</div>
    <div class="btn zero" @click="append('0')">0</div>
    <div class="btn" @click="dot">.</div>
    <div class="btn operator" @click="equal">=</div>
  </div>
</template>

<script>
export default {
  name: "Calculator",
  data() {
    return {
      previous: null,
      current: '',
      operator: null,
      operatorClicked: false
    };
  },
  methods: {
    clear() {
      this.current = '';
    },
    sign() {
      if(this.current != 0) {
      // check if character at first position is a minus, then delete if necessary or add
      this.current =
        this.current.charAt(0) === '-'
          ? this.current.slice(1)
          : `-${this.current}`;
      }
    },
    percent() {
      if (this.current != '') {
      // string to number and calc
      this.current = `${parseFloat(this.current) / 100}`;
      }
    },
    append(number) {
      if (this.operatorClicked) {
        this.current = '';
        this.operatorClicked = false;
      }

      this.current = `${this.current}${number}`;
    },
    dot() {
      // check if there is no dot yet
      if (this.current.indexOf('.') === -1) {
        this.append('.');
      }
    },
    setPrevious() {
      this.previous = this.current;
      this.operatorClicked = true;
    },
    divide() {
      this.operator = (a, b) => a / b;
      this.setPrevious();
    },
    times() {
      this.operator = (a, b) => a * b;
      this.setPrevious();
    },
    minus() {
      this.operator = (a, b) => a - b;
      this.setPrevious();
    },
    add() {
      this.operator = (a, b) => a + b;
      this.setPrevious();
    },
    equal() {
      this.current = `${this.operator(parseFloat(this.previous), parseFloat(this.current))}`;
      this.previous = null;
    }
  }
};
</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped>
.calculator {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
  font-size: 40px;
  width: 400px;
  margin: 0 auto;
  outline-style: double;
}

.display {
  grid-column: 1 / 5;
  background-color: #333;
  color: white;
  text-align: right;
  padding: 10px;
  font-weight: bold;
}

.btn {
  background-color: #f2f2f2;
  border: 1px solid #999;
}

.adjust {
  background-color: #d0cece;
}

.operator {
  background-color: #ffc65f;
  color: white; /* Will override color (regardless of order) */
  -webkit-text-stroke: 1px black;
   text-shadow: 1px 1px 2px #000;
  border: 1px solid #333;
}

.zero {
  grid-column: 1 / 3;
}
</style>
