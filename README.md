<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Calculator</title>
  <style>
    body {
      font-family: 'Roboto Mono', monospace;
      background-color: #f0f0f0;
      margin: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      flex-direction: column;
    }

    .calculator {
      background-color: #2b2b2b;
      border-radius: 10px;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
      padding: 20px;
      display: flex;
      flex-direction: column;
      align-items: center;
      width: 350px;
    }

    #result {
      width: 100%;
      height: 60px;
      font-size: 1.8rem;
      text-align: right;
      border: none;
      border-radius: 5px;
      padding: 5px;
      background-color: #1c1c1c;
      color: #e0e0e0;
      font-weight: bold;
      box-shadow: inset 0 2px 5px rgba(0, 0, 0, 0.5);
    }

    #company-name {
      font-size: 1.5rem;
      font-weight: bold;
      color: #ffa500;
      text-align: center;
      margin-top: 10px;
      text-shadow: 0 2px 5px rgba(0, 0, 0, 0.3);
      margin-bottom: 20px;
    }

    .buttons {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      grid-gap: 10px;
      width: 100%;
    }

    .btn {
      padding: 15px;
      font-size: 1.3rem;
      font-weight: bold;
      border: none;
      border-radius: 5px;
      background-color: #4a4a4a;
      color: #ffffff;
      cursor: pointer;
      transition: background-color 0.3s, transform 0.1s ease;
    }

    .btn:hover {
      background-color: #ff8c00;
      transform: scale(1.1);
    }

    .btn.operator {
      background-color: #757575;
    }

    .btn.operator:hover {
      background-color: #ffaa00;
    }

    .btn.equals {
      background-color: #ff5722;
    }

    .btn.equals:hover {
      background-color: #ff7043;
    }

    .btn.backspace {
      background-color: #616161;
    }

    .btn.backspace:hover {
      background-color: #757575;
    }

    .btn.decimal {
      background-color: #4a90e2;
    }

    .btn.decimal:hover {
      background-color: #5b9bd5;
    }
  </style>
</head>
<body>
  <div class="calculator">
    <div class="display">
      <input type="text" id="result" readonly value="0" />
    </div>
    <div id="company-name">Kingsio WD-100</div>
    <div class="buttons">
      <button class="btn" onclick="appendSymbol('7')">7</button>
      <button class="btn" onclick="appendSymbol('8')">8</button>
      <button class="btn" onclick="appendSymbol('9')">9</button>
      <button class="btn operator" onclick="setOperator('*')">×</button>

      <button class="btn" onclick="appendSymbol('4')">4</button>
      <button class="btn" onclick="appendSymbol('5')">5</button>
      <button class="btn" onclick="appendSymbol('6')">6</button>
      <button class="btn operator" onclick="setOperator('/')">÷</button>

      <button class="btn" onclick="appendSymbol('1')">1</button>
      <button class="btn" onclick="appendSymbol('2')">2</button>
      <button class="btn" onclick="appendSymbol('3')">3</button>
      <button class="btn" onclick="clearAll()">Clear</button>

      <button class="btn" onclick="appendSymbol('0')">0</button>
      <button class="btn decimal" id="decimalBtn" onclick="appendDecimal()">.</button>
      <button class="btn operator" onclick="setOperator('+')">+</button>
      <button class="btn operator" onclick="setOperator('-')">−</button>

      <button class="btn equals" onclick="calculate()">Enter</button>
      <button class="btn backspace" onclick="backspace()">←</button>
    </div>
  </div>

  <script>
    let resultField = document.getElementById('result');
    let decimalBtn = document.getElementById('decimalBtn');
    let firstNumber = null;
    let secondNumber = null;
    let currentOperator = null;
    let resetDisplay = false;

    function add(a, b) {
      return a + b;
    }

    function subtract(a, b) {
      return a - b;
    }

    function multiply(a, b) {
      return a * b;
    }

    function divide(a, b) {
      return b === 0 ? 'Error' : a / b;
    }

    function operate(operator, num1, num2) {
      switch (operator) {
        case '+':
          return add(num1, num2);
        case '-':
          return subtract(num1, num2);
        case '*':
          return multiply(num1, num2);
        case '/':
          return divide(num1, num2);
        default:
          return null;
      }
    }

    function appendSymbol(symbol) {
      if (resetDisplay) {
        resultField.value = '';
        resetDisplay = false;
      }
      resultField.value = resultField.value === '0' ? symbol : resultField.value + symbol;
      enableDecimal();
    }

    function clearAll() {
      resultField.value = '0';
      firstNumber = null;
      secondNumber = null;
      currentOperator = null;
      resetDisplay = false;
      enableDecimal();
    }

    function setOperator(operator) {
      if (currentOperator !== null && !resetDisplay) {
        calculate();
      }
      firstNumber = parseFloat(resultField.value);
      currentOperator = operator;
      resetDisplay = true;
    }

    function calculate() {
      if (currentOperator === null || firstNumber === null) return;
      secondNumber = parseFloat(resultField.value);
      const result = operate(currentOperator, firstNumber, secondNumber);
      resultField.value = `${firstNumber} ${currentOperator} ${secondNumber} = ${parseFloat(result.toFixed(6))}`;
      firstNumber = result;
      secondNumber = null;
      currentOperator = null;
      resetDisplay = true;
      enableDecimal();
    }

    function backspace() {
      let currentValue = resultField.value;
      if (currentValue.length > 1) {
        resultField.value = currentValue.slice(0, -1);
      } else {
        resultField.value = '0';
      }
      enableDecimal();
    }

    function appendDecimal() {
      if (!resultField.value.includes('.')) {
        resultField.value += '.';
        enableDecimal();
      }
    }

    function enableDecimal() {
      decimalBtn.disabled = resultField.value.includes('.');
    }

    document.addEventListener('keydown', function(event) {
      if (event.key >= '0' && event.key <= '9') {
        appendSymbol(event.key);
      } else if (event.key === '+') {
        setOperator('+');
      } else if (event.key === '-') {
        setOperator('-');
      } else if (event.key === '*') {
        setOperator('*');
      } else if (event.key === '/') {
        setOperator('/');
      } else if (event.key === 'Enter' || event.key === '=') {
        calculate();
      } else if (event.key === 'Backspace') {
        backspace();
      } else if (event.key === '.') {
        appendDecimal();
      }
    });
  </script>
</body>
</html>
