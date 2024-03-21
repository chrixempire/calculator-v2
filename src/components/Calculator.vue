<template>
  <div class="overall">
    <button @click="toggleBtnColorChange">Toggle</button>

    <div class="calculator">
      <div class="display" :class="{ 'toggle-display': !btnColorChange }">
        <p>{{ calculatorValue || "0" }}</p>
      </div>
      <div
        @click="clear"
        class="btn secondary"
        :class="{ 'toggle-secondary': !btnColorChange }"
      >
        AC
      </div>
      <div
        @click="sign"
        class="btn secondary"
        :class="{ 'toggle-secondary': !btnColorChange }"
      >
        ±
      </div>
      <div
        @click="percent"
        class="btn secondary"
        :class="{ 'toggle-secondary': !btnColorChange }"
      >
        %
      </div>
      <div
        @click="divide"
        class="btn operator"
        :class="{ 'toggle-operator': !btnColorChange }"
      >
        ÷
      </div>
      <div @click="append('7')" class="btn" :class="{ 'toggle-btn': !btnColorChange }">
        7
      </div>
      <div @click="append('8')" class="btn" :class="{ 'toggle-btn': !btnColorChange }">
        8
      </div>
      <div @click="append('9')" class="btn" :class="{ 'toggle-btn': !btnColorChange }">
        9
      </div>
      <div
        @click="multiply"
        class="btn operator"
        :class="{ 'toggle-operator': !btnColorChange }"
      >
        x
      </div>
      <div @click="append('4')" class="btn" :class="{ 'toggle-btn': !btnColorChange }">
        4
      </div>
      <div @click="append('5')" class="btn" :class="{ 'toggle-btn': !btnColorChange }">
        5
      </div>
      <div @click="append('6')" class="btn" :class="{ 'toggle-btn': !btnColorChange }">
        6
      </div>
      <div
        @click="minus"
        class="btn operator"
        :class="{ 'toggle-operator': !btnColorChange }"
      >
        -
      </div>
      <div @click="append('1')" class="btn" :class="{ 'toggle-btn': !btnColorChange }">
        1
      </div>
      <div @click="append('2')" class="btn" :class="{ 'toggle-btn': !btnColorChange }">
        2
      </div>
      <div @click="append('3')" class="btn" :class="{ 'toggle-btn': !btnColorChange }">
        3
      </div>
      <div
        @click="Add"
        class="btn operator"
        :class="{ 'toggle-operator': !btnColorChange }"
      >
        +
      </div>
      <div
        @click="append('0')"
        class="btn zero"
        :class="{ 'toggle-btn': !btnColorChange }"
      >
        0
      </div>
      <div @click="point" class="btn" :class="{ 'toggle-btn': !btnColorChange }">.</div>
      <div
        @click="equal"
        class="btn operator"
        :class="{ 'toggle-operator': !btnColorChange }"
      >
        =
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      previous: null,
      //   current: "",
      calculatorValue: "",
      operator: null,
      operatorClicked: false,
      btnColorChange: true,
    };
  },
  methods: {
    toggleBtnColorChange() {
      this.btnColorChange = !this.btnColorChange;
      console.log("work", this.btnColorChange);
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
      this.calculatorValue = `${this.operator(
        parseFloat(this.previousValue),
        parseFloat(this.calculatorValue)
      )}`;
    },
    percent() {
      this.calculatorValue = parseFloat(this.calculatorValue) / 100;
    },
  },
};
</script>

<style scoped>
.calculator {
  margin: 0 auto;
  width: 400px;
  font-size: 30px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
}

button {
  padding: 10px 20px;
  background: black;
  width: auto;
  outline: none;
  border: 0px;
  margin-bottom: 10px;
  border-radius: 4px;
  color: white;
  font-size: 15px;
}
.display {
  height: 150px;

  grid-column: 1 / 5;
  background-color: rgb(66, 25, 25);

  color: white;
  text-align: right;
  display: flex;
  justify-content: flex-end;
  align-items: center;
  padding: 0px 10px;
}
p {
  font-size: 80px;
  font-weight: 200;
}
.zero {
  grid-column: 1 / 3;
  height: 70px;
}

.btn {
  background-color: rgb(50, 30, 0, 0.4);
  height: 100px;
  border: 1px solid rgb(37, 33, 33);
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
}
.btn:hover {
  background-color: rgb(50, 30, 0, 0.2);
}
.secondary {
  background-color: rgb(50, 30, 0, 0.6);
}
.secondary:hover {
  background-color: rgb(50, 30, 0, 0.4);
}
.operator {
  background-color: rgb(255, 159, 12);
  color: white;
}
.operator:hover {
  background-color: rgba(255, 159, 12, 0.8);
}
.btn,
.operator {
  cursor: pointer;
}

.toggle-operator {
  background-color: #cc0000;
}
.toggle-operator:hover {
  background-color: rgb(204, 0, 0, 0.8);
}
.toggle-secondary {
  background-color: rgba(0, 0, 0, 0.3);
}
.toggle-btn {
  background-color: rgba(0, 0, 0, 0.1);
}
.toggle-display {
  background-color: rgb(71, 69, 69);
}
</style>
