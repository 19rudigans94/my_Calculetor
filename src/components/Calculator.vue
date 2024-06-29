<template>
  <div class="calculator">
    <div class="display" contenteditable="true" @input="handleInput">{{ display }}</div>
    <div class="buttons">
      <button v-for="(button, index) in buttons" :key="index" @click="handleButtonClick(button)"
        :class="{ operator: isOperator(button) }">{{ button.text }}</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      display: '0',
      operand1: null,
      operand2: null,
      operator: null,
      awaitingOperand2: false,
      buttons: [
        { text: '7', value: '7' },
        { text: '8', value: '8' },
        { text: '9', value: '9' },
        { text: '/', value: '/' },
        { text: '4', value: '4' },
        { text: '5', value: '5' },
        { text: '6', value: '6' },
        { text: '*', value: '*' },
        { text: '1', value: '1' },
        { text: '2', value: '2' },
        { text: '3', value: '3' },
        { text: '-', value: '-' },
        { text: '0', value: '0' },
        { text: '.', value: '.' },
        { text: 'C', value: 'C' },
        { text: '+', value: '+' },
        { text: '=', value: '=' },
      ],
    };
  },
  methods: {
    handleButtonClick(button) {
      switch (button.value) {
        case 'C':
          this.clearDisplay();
          break;
        case '=':
          this.calculate();
          break;
        default:
          this.appendToDisplay(button.value);
          if (['/', '*', '-', '+'].includes(button.value)) {
            this.setOperation(button.value);
          }
      }
    },
    appendToDisplay(char) {
      if (this.display === '0' || this.awaitingOperand2) {
        this.display = char;
        this.awaitingOperand2 = false;
      } else {
        if (char === '.') {
          if (this.display.indexOf('.') === -1) {
            this.display += char;
          }
        } else {
          this.display += char;
        }
      }
    },
    clearDisplay() {
      this.display = '0';
      this.operand1 = null;
      this.operand2 = null;
      this.operator = null;
      this.awaitingOperand2 = false;
    },
    setOperation(op) {
      if (this.operator && this.awaitingOperand2) {
        this.calculate();
      }
      this.operator = op;
      this.operand1 = parseFloat(this.display);
      this.awaitingOperand2 = true;
      this.display = '0';
    },
    calculate() {
      if (this.operator && this.operand1 !== null && this.operand2 !== null) {
        const num1 = parseFloat(this.operand1);
        const num2 = parseFloat(this.operand2);
        const result = this.performCalculation(num1, num2, this.operator);
        this.display = isNaN(result) ? 'Error' : result.toString();
        this.operand1 = result.toString();
        this.operand2 = null;
        this.operator = null;
        this.awaitingOperand2 = false;
      }
    },
    performCalculation(operand1, operand2, operator) {
      switch (operator) {
        case '+':
          return operand1 + operand2;
        case '-':
          return operand1 - operand2;
        case '*':
          return operand1 * operand2;
        case '/':
          return operand2 !== 0 ? operand1 / operand2 : 'Error';
        default:
          return 0;
      }
    },
    handleInput(event) {
      let value = event.target.innerText;
      value = value.replace(/[^\d.]/g, '');
      if (value.split('.').length > 2) {
        value = value.slice(0, -1);
      }
      this.display = value === '' ? '0' : value;
    },
    handleKeyDown(event) {
      switch (event.key) {
        case 'Enter':
          this.calculate();
          break;
        case '.':
          this.appendToDisplay('.');
          break;
        case 'C':
          this.clearDisplay();
          break;
        case '+':
        case '-':
        case '*':
        case '/':
          this.setOperation(event.key);
          break;
        default:
          if (!isNaN(event.key)) {
            this.appendToDisplay(event.key);
          }
      }
    },
  },
  computed: {
    isOperator() {
      return (button) => ['/', '*', '-', '+', '='].includes(button.value);
    },
  },
  mounted() {
    document.addEventListener('keyup', this.handleKeyDown);
  },
  beforeDestroy() {
    document.removeEventListener('keyup', this.handleKeyDown);
  },
};
</script>

<style scoped>
.calculator {
  width: 300px;
  margin: 20px auto;
  border: 1px solid #ccc;
  padding: 10px;
  border-radius: 10px;
  background-color: #f4f4f4;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
}

.display {
  font-size: 36px;
  text-align: right;
  padding: 20px;
  background-color: #eaeaea;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-bottom: 10px;
  box-shadow: inset 0px 0px 5px rgba(0, 0, 0, 0.1);
  min-height: 60px;
  overflow: hidden;
}

.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 10px;
}

.buttons button {
  font-size: 24px;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  cursor: pointer;
  background-color: #f0f0f0;
  transition: background-color 0.3s ease;
}

.buttons button:hover {
  background-color: #e0e0e0;
}

.buttons .operator {
  color: #007aff;
  background-color: #f0f0f0;
}

.buttons .operator:hover {
  background-color: #d4e4ff;
}

.buttons button:active {
  background-color: #ccc;
}
</style>
