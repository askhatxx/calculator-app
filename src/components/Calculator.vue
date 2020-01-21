<template>
  <div class="calculator">
    <div class="display">
      <div class="answer">Ans = -0.123</div>
      <div class="current">-0.123</div>
    </div>
    <button class="btn keys">←</button>
    <button class="btn keys">AC</button>
    <button class="btn keys">+/-</button>
    <button class="btn keys">Copy</button>
    <button class="btn">7</button>
    <button class="btn">8</button>
    <button class="btn">9</button>
    <button class="btn operator">÷</button>
    <button class="btn">4</button>
    <button class="btn">5</button>
    <button class="btn">6</button>
    <button class="btn operator">×</button>
    <button class="btn">1</button>
    <button class="btn">2</button>
    <button class="btn">3</button>
    <button class="btn operator">−</button>
    <button class="btn">0</button>
    <button class="btn dot">.</button>
    <button class="btn equal">=</button>
    <button class="btn operator">+</button>
  </div>
</template>

<script>
export default {}
</script>

<style scoped>
.calculator {
  background: #fff;
  width: 90%;
  max-width: 350px;
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
  font-size: 30px;
  color: #222;
  font-weight: normal;
  user-select: text;
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
