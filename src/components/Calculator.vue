<template>
  <div class="overall-container">
    <div class="top-detail">
      <Togglebtn :checked="btnColorChange" @click="toggleBtnColorChange" />
      <div v-if="showAlert" class="alert-message">
        <p>Maximum of 9 characters allowed!</p>
      </div>
    </div>

    <div class="calculator" :class="{ theme: !btnColorChange }">
      <div class="displayScreen">
        <p>{{ calculatorValue }}</p>
      </div>
      <div @click="clear" class="btn specialbtn">AC</div>
      <div @click="sign" class="btn specialbtn">±</div>
      <div @click="percent" class="btn specialbtn">%</div>
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
      // Initial value of the calculator display
      calculatorValue: "0",
      // Function to perform arithmetic operations
      operator: null,
      // Flag to indicate if an operator button was clicked
      operatorClicked: false,
      // Flag to toggle button color change
      btnColorChange: false,
      // Flag to show/hide the alert message
      showAlert: false,
    };
  },
  components: {
    Togglebtn,
  },
  // Watcher to monitor changes in the calculatorValue
  watch: {
    calculatorValue(newValue) {
      const strippedValue = newValue.replace(/[-.]/g, "");
      if (strippedValue.length > 9) {
        this.showAlert = true;
        setTimeout(() => {
          this.showAlert = false;
        }, 3000);
        this.calculatorValue = newValue.slice(0, 9);
      } else {
        const decimalIndex = newValue.indexOf(".");
        if (strippedValue.length === 9 && decimalIndex !== -1) {
          this.showAlert = true;
          setTimeout(() => {
            this.showAlert = false;
          }, 3000);
          this.calculatorValue = newValue.slice(0, 9);
        }
      }
    },
  },
  methods: {
    // Method to toggle button color change
    toggleBtnColorChange() {
      this.btnColorChange = !this.btnColorChange;
    },
    // Method to append a number to the calculator display
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
    // Method to handle the decimal point button
    point() {
      if (this.operatorClicked || this.calculatorValue === "0") {
        this.calculatorValue = "0.";
        this.operatorClicked = false;
      } else if (!this.calculatorValue.includes(".")) {
        this.calculatorValue += ".";
      }
    },
    // Method to clear the calculator display
    clear() {
      this.calculatorValue = "0";
      this.operatorClicked = false;
    },
    // Method to set the previous value for arithmetic operations
    setPrevious() {
      this.previousValue = this.calculatorValue;
      this.operatorClicked = true;
    },
    // Method to handle the addition operation
    Add() {
      this.operator = (a, b) => a + b;
      this.setPrevious();
    },
    // Method to handle the multiplication operation
    multiply() {
      this.operator = (a, b) => a * b;
      this.setPrevious();
    },
    // Method to handle the division operation
    divide() {
      this.operator = (a, b) => a / b;
      this.setPrevious();
    },
    // Method to handle the subtraction operation
    minus() {
      this.operator = (a, b) => a - b;
      this.setPrevious();
    },
    // Method to toggle the sign of the calculator display
    sign() {
      this.calculatorValue =
        this.calculatorValue.charAt(0) === "-"
          ? this.calculatorValue.slice(1)
          : `-${this.calculatorValue}`;
    },
    // Method to perform the arithmetic operation and display the result
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
    // Method to calculate the percentage of the current value
    percent() {
      let result;
      if (this.calculatorValue === "0") {
        result = 0;
      } else {
        result = parseFloat(this.calculatorValue) / 100;
      }
      if (Number.isFinite(result)) {
        let roundedResult = parseFloat(result.toFixed(7));
        let resultString = roundedResult.toString();
        resultString = resultString.replace(/\.?0*$/, "");
        this.calculatorValue = resultString === "" ? "0" : resultString;
      } else {
        this.calculatorValue = "Error";
      }
      this.setPrevious();
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

.displayScreen {
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
.displayScreen p {
  font-size: 80px;
  font-family: "Roboto", sans-serif;
  font-weight: 100;
  font-style: normal;
  height: 100%;
  width: 100%;
  overflow: hidden;
  white-space: nowrap;
  margin-top: 50px;
}
.alert-message p {
  color: rgba(203, 26, 20, 1);
}
.top-detail {
  display: flex;
  align-items: center;
  gap: 20px;
  width: 100%;
  height: 40px;
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
.specialbtn {
  background-color: var(--specialbtn-bg-color);
}
.specialbtn:hover {
  background-color: var(--specialbtn-bg-hover-color);
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

/* For Responsiveness of different screens */

@media screen and (max-width: 550px) {
  .overall-container {
    display: flex;
    max-width: 400px;
  }
  .calculator {
    font-size: 25px;
  }
  .btn {
    height: 75px;
  }
  .zero {
    height: 75px;
  }
  .btn:hover {
    background-color: var(--bg-hover-color);
  }
  .specialbtn {
    background-color: var(--specialbtn-bg-color);
  }
  .specialbtn:hover {
    background-color: var(--specialbtn-bg-hover-color);
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
}
@media screen and (max-width: 450px) {
  .displayScreen p {
    font-size: 70px;
  }
  .displayScreen {
    height: 135px;
  }
}
@media screen and (max-width: 380px) {
  .displayScreen p {
    font-size: 65px;
  }

  .displayScreen {
    height: 125px;
  }
}
@media screen and (max-width: 360px) {
  .displayScreen p {
    font-size: 60px;
  }
}
@media screen and (max-width: 325px) {
  .displayScreen p {
    font-size: 55px;
  }
  .displayScreen {
    height: 115px;
  }
}
@media screen and (max-width: 305px) {
  .displayScreen p {
    font-size: 50px;
  }
}
@media screen and (max-width: 285px) {
  .displayScreen p {
    font-size: 45px;
  }
  .displayScreen {
    height: 105px;
  }
}
@media screen and (max-width: 255px) {
  .displayScreen p {
    font-size: 40px;
  }
}
</style>
