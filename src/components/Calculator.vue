<template>
  <div class="overall-container">
    <div>
      <Togglebtn :checked="btnColorChange" @click="toggleBtnColorChange" />
    </div>
    <div class="calculator" :class="{ theme: !btnColorChange }">
      <div class="display">
        <p>{{ calculatorValue || "0" }}</p>
      </div>
      <div @click="clear" class="btn secondary">AC</div>
      <div @click="sign" class="btn secondary">±</div>
      <div @click="percent" class="btn secondary">%</div>
      <div @click="divide" class="btn operator">÷</div>
      <div @click="append('7')" class="btn">7</div>
      <div @click="append('8')" class="btn">8</div>
      <div @click="append('9')" class="btn">9</div>
      <div @click="multiply" class="btn operator">x</div>
      <div @click="append('4')" class="btn">4</div>
      <div @click="append('5')" class="btn">5</div>
      <div @click="append('6')" class="btn">6</div>
      <div @click="minus" class="btn operator">-</div>
      <div @click="append('1')" class="btn">1</div>
      <div @click="append('2')" class="btn">2</div>
      <div @click="append('3')" class="btn">3</div>
      <div @click="Add" class="btn operator">+</div>
      <div @click="append('0')" class="btn zero">0</div>
      <div @click="point" class="btn">.</div>
      <div @click="equal" class="btn operator">=</div>
    </div>
  </div>
</template>

<script>
import Togglebtn from "./Togglebtn.vue";
export default {
  data() {
    return {
      previous: null,
      //   current: "",
      calculatorValue: "",
      operator: null,
      operatorClicked: false,
      btnColorChange: false,
    };
  },
  components: {
    Togglebtn,
  },
  methods: {
    toggleBtnColorChange() {
      this.btnColorChange = !this.btnColorChange;
    },
    append(number) {
      if (this.operationClicked) {
        this.calculatorValue = "";
        this.operationClicked = false;
      }
      this.calculatorValue = `${this.calculatorValue}${number}`;
    },

    point() {
      if (this.calculatorValue.indexOf(".") === -1) {
        this.append(".");
      }
    },
    del() {
      this.calculatorValue = this.calculatorValue.slice(0, -1);
    },
    clear() {
      this.calculatorValue = "";
    },
    setPrevious() {
      this.previousValue = this.calculatorValue;
      this.operationClicked = true;
    },
    Add() {
      this.operator = (a, b) => a + b;
      this.setPrevious();
    },
    multiply() {
      this.operator = (a, b) => a * b;
      this.setPrevious();
    },
    divide() {
      this.operator = (a, b) => a / b;
      this.setPrevious();
    },
    minus() {
      this.operator = (a, b) => a - b;
      this.setPrevious();
    },
    sign() {
      this.calculatorValue =
        this.calculatorValue.charAt(0) === "-"
          ? this.calculatorValue.slice(1)
          : `-${this.calculatorValue}`;
    },
    equal() {
      if (this.operator && this.calculatorValue !== "") {
        let result = this.operator(
          parseFloat(this.previousValue),
          parseFloat(this.calculatorValue)
        );
        if (Number.isFinite(result) && result % 1 !== 0) {
          let resultString = result.toString();
          let decimalIndex = resultString.indexOf(".");
          if (decimalIndex !== -1 && resultString.length - decimalIndex > 7) {
            this.calculatorValue = result.toFixed(7);
          } else {
            this.calculatorValue = `${result}`;
          }
        } else {
          this.calculatorValue = `${result}`;
        }

        this.previousValue = null;
        this.operator = null;
      }
      this.operationClicked = true;
    },

    percent() {
      let result = parseFloat(this.calculatorValue) / 100;
      if (Number.isFinite(result) && result % 1 !== 0) {
        let resultString = result.toString();
        let decimalIndex = resultString.indexOf(".");
        if (decimalIndex !== -1 && resultString.length - decimalIndex > 7) {
          this.calculatorValue = result.toFixed(7);
        } else {
          this.calculatorValue = `${result}`;
        }
      } else {
        this.calculatorValue = `${result}`;
      }
    },
  },
};
</script>

<style>
:root {
  --bg-color: rgb(66, 25, 25);
  --bg-hover-color: rgb(50, 30, 0, 0.2);
  --btn-bg-color: rgb(50, 30, 0, 0.4);
  --secondary-bg-color: rgb(50, 30, 0, 0.6);
  --secondary-bg-hover-color: rgb(50, 30, 0, 0.4);
  --operator-bg-color: rgb(255, 159, 12);
  --operator-bg-hover-color: rgba(255, 159, 12, 0.8);
  --display-text-color: white;
  --btn-text-color: white;
}
.theme {
  --display-text-color: white;
  --bg-color: rgb(71, 69, 69);
  --bg-hover-color: rgba(0, 0, 0, 0.04);
  --btn-bg-color: rgba(0, 0, 0, 0.1);
  --btn-text-color: black;
  --secondary-bg-color: rgba(0, 0, 0, 0.3);
  --secondary-bg-hover-color: rgba(0, 0, 0, 0.1);
  --operator-bg-color: #cc0000;
  --operator-bg-hover-color: rgb(204, 0, 0, 0.8);
}
</style>

<style scoped>
.overall-container {
  display: flex;
  max-width: 400px;
  width: 100%;
  justify-content: center;
  align-items: flex-start;
  flex-direction: column;
  gap: 15px;
  margin: auto;
  padding: 15px;
  border-radius: 12px;
  box-shadow: 0px 20px 24px rgba(71, 83, 103, 0.15);
}
.calculator {
  margin: 0 auto;
  width: 100%;
  font-size: 30px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
}

.display {
  height: 125px;
  grid-column: 1 / 5;
  background-color: var(--bg-color);
  color: var(--display-text-color);
  text-align: right;
  display: flex;
  justify-content: flex-end;
  align-items: flex-start;
  padding-right: 15px;
}
.display p {
  font-size: 80px;
  font-family: "Roboto", sans-serif;
  font-weight: 100;
  font-style: normal;
  height: 100%;
  margin-top: 30px;
}

.btn {
  background-color: var(--btn-bg-color);
  height: 100px;
  color: var(--btn-text-color);
  border: 1px solid rgb(37, 33, 33);
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}
.zero {
  grid-column: 1 / 3;
  height: 100px;
  text-align: left;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  padding-left: 30px;
}
.btn:hover {
  background-color: var(--bg-hover-color);
}
.secondary {
  background-color: var(--secondary-bg-color);
}
.secondary:hover {
  background-color: var(--secondary-bg-hover-color);
}
.operator {
  background-color: var(--operator-bg-color);
}
.operator:hover {
  background-color: var(--operator-bg-hover-color);
}
.operator {
  cursor: pointer;
}
</style>
