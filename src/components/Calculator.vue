import Vue from 'vue'
<template>
  <div class="calc">
    {{ message }}

    <div>
      <input type="number" placeholder="Operand1" v-model.number="operand1" />

      <input type="number" placeholder="Operand2" v-model.number="operand2" />
      = {{ result }}
      <div class="error" v-if="error">Ошибка! {{ error }}</div>
      <div class="keyboard">
        <button
          v-for="operation in operations"
          :key="operation"
          @click="calculate(operation)"
          :title="operation"
          :disabled="operand1 === '' || operand2 === ''"
        >
          {{ operation }}
        </button>
      </div>
    </div>
    <label><input type="checkbox" v-model="show">Экранная клавиатура</label>
    <div class="keyboard-screen" v-if="show">       
      <button v-for="number in numbers" :key="number" @click="inputNum(number)">{{number}}</button>
      <button @click="delNumber">delete</button>
      <br>
      <label><input type="radio" value="operand1" v-model="operand">операнд1</label>
      <label><input type="radio" value="operand2" v-model="operand">операнд2</label>      
    </div>
    
    
    <div class="logs">
      <div v-for="(log, id) in logs" v-bind:key="id">{{ log }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Calculator",
  data: () => ({
    operand: "",
    message: "",
    operand1: 0,
    operand2: 0,
    result: 0,
    error: "",
    show: false,
    operations: ["+", "-", "*", "/", "**", "//"],
    logs: {}, //логи операций
    numbers: [1, 2, 3, 4, 5, 6, 7, 8, 9, 0],
    
  }),
  methods: {
    delNumber() {
      this[this.operand] = +String(this[this.operand]).slice(0,-1)
    },
    inputNum(number) {
      this[this.operand] = +[this[this.operand] += String(number)]
    },
    calculate(operation = "+") {
      
      this.error = "";
      switch (operation) {
        case "+":
          this.sum();
          break;
        case "-":
          this.sub();
          break;
        case "*":
          this.mul();
          break;
        case "/":
          this.div();
          break;
        case "**":
          this.pow();
          break;
        case "//":
          this.int_div();
          break;
      }
      const key = Date.now();
      const value = `${this.operand1}${operation}${this.operand2}=${this.result}`;
      this.$set(this.logs, key, value);
    },
    sum() {
      this.result = this.operand1 + this.operand2;
    },
    sub() {
      this.result = this.operand1 - this.operand2;
    },
    mul() {
      this.result = this.operand1 * this.operand2;
    },
    div() {
      const { operand1, operand2 } = this;
      if (operand2 === 0) {
        this.error = "На 0 делить нельзя";
      } else {
        this.result = operand1 / operand2;
      }
    },
    pow() {
      this.result = this.operand1 ** this.operand2;
    },
    int_div() {
      const { operand1, operand2 } = this;
      if (operand2 === 0) {
        this.error = "На 0 делить нельзя";
      } else {
        this.result = (operand1 - (operand1 % operand2)) / operand2;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.error {
  color: red;
}
.keyboard {
  margin-top: 20px;
}
.keyboard-screen {
  margin-top: 20px;
}
</style>