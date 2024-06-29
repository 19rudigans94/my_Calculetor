<template>
  <div id="calculator">
    <a-input :value="currentInput" class="display" disabled />
    <a-input :value="currentOperation" class="operation" disabled />
    <a-row gutter="8" class="buttons">
      <a-col :span="6" :style="{ marginBottom: '8px' }"><a-button @click="appendToInput('7')">7</a-button></a-col>
      <a-col :span="6" :style="{ marginBottom: '8px' }"><a-button @click="appendToInput('8')">8</a-button></a-col>
      <a-col :span="6" :style="{ marginBottom: '8px' }"><a-button @click="appendToInput('9')">9</a-button></a-col>
      <a-col :span="6" :style="{ marginBottom: '8px' }"><a-button type="primary"
          @click="setOperation('+')">+</a-button></a-col>
      <a-col :span="6" :style="{ marginBottom: '8px' }"><a-button @click="appendToInput('4')">4</a-button></a-col>
      <a-col :span="6" :style="{ marginBottom: '8px' }"><a-button @click="appendToInput('5')">5</a-button></a-col>
      <a-col :span="6" :style="{ marginBottom: '8px' }"><a-button @click="appendToInput('6')">6</a-button></a-col>
      <a-col :span="6" :style="{ marginBottom: '8px' }"><a-button type="primary"
          @click="setOperation('-')">-</a-button></a-col>
      <a-col :span="6" :style="{ marginBottom: '8px' }"><a-button @click="appendToInput('1')">1</a-button></a-col>
      <a-col :span="6" :style="{ marginBottom: '8px' }"><a-button @click="appendToInput('2')">2</a-button></a-col>
      <a-col :span="6" :style="{ marginBottom: '8px' }"><a-button @click="appendToInput('3')">3</a-button></a-col>
      <a-col :span="6" :style="{ marginBottom: '8px' }"><a-button type="primary"
          @click="setOperation('*')">*</a-button></a-col>
      <a-col :span="6" :style="{ marginBottom: '8px' }"><a-button @click="appendToInput('0')">0</a-button></a-col>
      <a-col :span="6" :style="{ marginBottom: '8px' }"><a-button @click="appendToInput('.')">.</a-button></a-col>
      <a-col :span="6" :style="{ marginBottom: '8px' }"><a-button type="primary"
          @click="calculate()">=</a-button></a-col>
      <a-col :span="6" :style="{ marginBottom: '8px' }"><a-button type="primary"
          @click="setOperation('/')">/</a-button></a-col>
      <a-col :span="6" :style="{ marginBottom: '8px' }"><a-button @click="clear()">C</a-button></a-col>
    </a-row>
  </div>
</template>

<script>
import { Input, Button, Row, Col } from 'ant-design-vue';

export default {
  components: {
    'a-input': Input,
    'a-button': Button,
    'a-row': Row,
    'a-col': Col,
  },
  data() {
    return {
      currentInput: '0',
      currentOperation: ''
    };
  },
  methods: {
    appendToInput(value) {
      if (this.currentInput === '0' && value !== '.') {
        this.currentInput = value;
      } else {
        this.currentInput += value;
      }
    },
    setOperation(operation) {
      this.currentOperation = operation;
      if (this.currentInput !== '0') {
        this.currentInput += ' ' + operation + ' ';
      }
    },
    calculate() {
      let expression = this.currentInput.replace(' ', '');
      let result = eval(expression);
      this.currentInput = result.toString();
      this.currentOperation = '';
    },
    clear() {
      this.currentInput = '0';
      this.currentOperation = '';
    }
  }
};
</script>

<style scoped>
#calculator {
  width: 240px;
  margin: 20px auto;
  border: 1px solid #ccc;
  padding: 10px;
  border-radius: 5px;
}

.display {
  text-align: right;
  margin-bottom: 10px;
}

.operation {
  text-align: right;
  color: #777;
  margin-bottom: 10px;
}

.buttons {
  text-align: center;
}
</style>
