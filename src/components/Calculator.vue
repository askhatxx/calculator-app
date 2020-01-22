<template>
  <div class="calculator">
    <div class="display">
      <div class="answer">{{ answer ? `Ans = ${answerNumber}` : `` }}</div>
      <div class="current" :class='{delete: isOperator}'>{{ current }}</div>
    </div>
    <button @click='backspace' class="btn keys">←</button>
    <button @click='allClear' class="btn keys">AC</button>
    <button @click='addSign' class="btn keys">+/-</button>
    <button class="btn keys">Copy</button>
    <button @click='addSymbol(`7`)' class="btn">7</button>
    <button @click='addSymbol(`8`)' class="btn">8</button>
    <button @click='addSymbol(`9`)' class="btn">9</button>
    <button @click='division' class="btn operator">÷</button>
    <button @click='addSymbol(`4`)' class="btn">4</button>
    <button @click='addSymbol(`5`)' class="btn">5</button>
    <button @click='addSymbol(`6`)' class="btn">6</button>
    <button @click='multiply' class="btn operator">×</button>
    <button @click='addSymbol(`1`)' class="btn">1</button>
    <button @click='addSymbol(`2`)' class="btn">2</button>
    <button @click='addSymbol(`3`)' class="btn">3</button>
    <button @click='minus' class="btn operator">−</button>
    <button @click='addSymbol(`0`)' class="btn">0</button>
    <button @click='addSymbol(`.`)' class="btn dot">.</button>
    <button @click='equal' class="btn equal">=</button>
    <button @click='plus' class="btn operator">+</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      answer: null,
      current: '0',
      operator: false,
      isOperator: false,
    }
  },
  computed: {
    currentNumber() {
      return Number(this.current);
    },
    answerNumber() {
      return Number(this.answer.toFixed(16));
    }
  },
  methods: {
    backspace() {
      this.current = this.current.slice(0, -1);
      if (this.current === '') this.current = '0';
      if (this.current === '-') this.current = '-0';
    },
    allClear() {
      this.answer = null;
      this.current = '0';
    },
    addSign() {
      this.current = this.current.charAt(0) === '-' ? this.current.slice(1) : `-${this.current}`;
    },
    addSymbol(symbol) {
      if (this.isOperator) this.current = '0';
      this.isOperator = false;

      if (this.current.length >= 15) return;
      if (symbol === '0' && (this.current === '0' || this.current === '-0')) return;
      if (symbol === '.' && this.current.includes('.')) return;

      if (this.current === '0' || this.current === '-0') {
        if (symbol !== '.') {
          this.current = `${this.current.slice(0, -1)}${symbol}`;
          return;
        }
      }
      this.current = `${this.current}${symbol}`;
    },
    operatorRun() {
      if (this.answer === null) {
        this.answer = this.currentNumber;
      }
      else {
        this.answer = this.operator(this.answerNumber, this.currentNumber);
      }
      this.isOperator = true;
    },
    plus() {
      this.operatorRun();
      this.operator = (a, b) => a + b;
    },
    minus() {
      this.operatorRun();
      this.operator = (a, b) => a - b;
    },
    multiply() {
      this.operatorRun();
      this.operator = (a, b) => a * b;
    },
    division() {
      this.operatorRun();
      this.operator = (a, b) => a / b;
    },
    equal() {
      if (this.answer !== null) {
        this.answer = this.operator(this.answerNumber, this.currentNumber);
        this.current = `${this.answerNumber}`;
        this.answer = null;
      }
    }
  }
}
</script>

<style scoped>
.calculator {
  background: #fff;
  width: 90%;
  max-width: 370px;
  border: 1px solid #dfe1e5;
  border-radius: 8px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 8px;
  padding: 16px;
}
.display {
  grid-column: 1 / 5;
  border: 1px solid #ebebeb;
  border-radius: 8px;
  margin-bottom: 8px;
  padding: 10px 14px 6px;
  text-align: right;
  overflow: hidden;
}
.answer {
  color:#70757a;
  font-size: 13px;
  height: 16px;
  margin-bottom: 4px;
}
.current {
  color: #222;
  font-size: 30px;
  height: 34px;
  font-weight: normal;
  user-select: text;
}
.current.delete {
  color: #afb1b7;
}
.btn {
  background: #f1f3f4;
  border: 1px solid #f1f3f4;
  border-radius: 4px;
  font-family: inherit;
  color: #202124;
  line-height: 34px;
  font-size: 14px;
  text-align: center;
  cursor: pointer;
  outline: none;
  user-select: none;
}
.btn:hover {
  background: #e8eaeb;
  border-color: #e8eaeb;
}
.btn:active {
  background: #e8eaeb;
  border-color: #bdc1c6;
}
.keys {
  background: #dfe1e5;
  border: 1px solid #dfe1e5;
  font-size: 13px;
}
.keys:hover {
  background: #d7d9dd;
  border-color: #d7d9dd;
}
.keys:active {
  background: #d7d9dd;
  border-color: #9aa0a6;
}
.operator {
  background: #dfe1e5;
  border: 1px solid #dfe1e5;
  font-size: 18px;
}
.operator:hover {
  background: #d7d9dd;
  border-color: #d7d9dd;
}
.operator:active {
  background: #d7d9dd;
  border-color: #9aa0a6;
}
.equal {
  background: #4285f4;
  border: 1px solid #4285f4;
  font-size: 18px;
  color: #fff;
  font-weight: bold;
}
.equal:hover {
  background: #4d8bf1;
  border-color: #4d8bf1;
}
.equal:active {
  background: #4d8bf1;
  border-color: #185abc;
  box-shadow: 0 1px 2px 0 rgba(66,133,244,0.45), 0 3px 6px 2px rgba(66,133,244,0.30);
}
.dot {
  font-size: 18px;
  font-weight: bold;
}
</style>
