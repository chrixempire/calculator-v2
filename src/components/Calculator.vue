<template>
  <div class="overall-container">
    <div>
      <Togglebtn :checked="btnColorChange" @click="toggleBtnColorChange" />
    </div>
    <div class="calculator" :class="{ theme: !btnColorChange }">
      <div class="display">
        <p>{{ calculatorValue }}</p>
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
      calculatorValue: "0", // Set initial value to "0"
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
      if (this.operatorClicked) {
        this.calculatorValue = "";
        this.operatorClicked = false;
      }
      if (this.calculatorValue === "0" && number !== ".") {
        this.calculatorValue = "";
      }
      this.calculatorValue += number;
    },
    point() {
      if (this.operatorClicked || this.calculatorValue === "0") {
        // If an operator was clicked or the display is "0", set display to "0."
        this.calculatorValue = "0.";
        this.operatorClicked = false;
      } else if (!this.calculatorValue.includes(".")) {
        this.calculatorValue += ".";
      }
    },

    clear() {
      this.calculatorValue = "0";
      this.operatorClicked = false;
    },
    setPrevious() {
      this.previousValue = this.calculatorValue;
      this.operatorClicked = true;
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
        if (Number.isFinite(result)) {
          if (result % 1 !== 0) {
            let roundedResult = parseFloat(result.toFixed(7));
            if (roundedResult % 1 === 0) {
              this.calculatorValue = `${parseInt(roundedResult, 10)}`;
            } else {
              this.calculatorValue = `${roundedResult}`;
            }
          } else {
            this.calculatorValue = `${result}`;
          }
        } else {
          this.calculatorValue = "Error";
        }
        this.previousValue = null;
        this.operator = null;
      }
      this.operatorClicked = true;
    },

    percent() {
      let result;
      if (
        this.calculatorValue === "" ||
        this.calculatorValue === "0" ||
        this.calculatorValue === "-"
      ) {
        result = 0;
      } else {
        result = parseFloat(this.calculatorValue) / 100;
      }
      if (Number.isFinite(result)) {
        let roundedResult = parseFloat(result.toFixed(7));
        let resultString = roundedResult.toString();
        resultString = resultString.replace(/\.?0*$/, "");
        this.calculatorValue = resultString;
      } else {
        this.calculatorValue = "Error";
      }
    },
  },
};
</script>

<style scoped>
@import url("@/assets/main.css");
.overall-container {
  display: flex;
  max-width: 420px;
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
  height: 145px;
  grid-column: 1 / 5;
  background-color: var(--bg-color);
  color: var(--display-text-color);
  text-align: right;
  display: flex;
  justify-content: flex-end;
  align-items: flex-start;
  padding-right: 10px;
}
.display p {
  font-size: 80px;
  font-family: "Roboto", sans-serif;
  font-weight: 100;
  font-style: normal;
  height: 100%;
  margin-top: 50px;
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

@media screen and (max-width: 550px) {
  .overall-container {
    margin-bottom: 50px;
    
  }
}
</style>
