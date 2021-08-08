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
    <div class="logs">
    <div v-for="(log, id) in logs" v-bind:key="id">{{ log }}</div>
  </div>
  </div>
  
</template>

<script>
export default {
  name: "Calculator",
  data: () => ({
    message: "",
    operand1: 0,
    operand2: 0,
    result: 0,
    error: "",
    operations: ["+", "-", "*", "/", "**", "//"],
    logs: {}, //логи операций
  }),
  methods: {
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
</style>