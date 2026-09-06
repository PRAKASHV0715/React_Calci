# Ex04 Simple Calculator - React Project
## Date:28-08-2026
## Name : Prakash v
## Reg No :212225230211

## AIM
To  develop a Simple Calculator using React.js with clean and responsive design, ensuring a smooth user experience across different screen sizes.

## ALGORITHM
### STEP 1
Create a React App.

### STEP 2
Open a terminal and run:
  <ul><li>npx create-react-app simple-calculator</li>
  <li>cd simple-calculator</li>
  <li>npm start</li></ul>

### STEP 3
Inside the src/ folder, create a new file Calculator.js and define the basic structure.

### STEP 4
Plan the UI: Display screen, number buttons (0-9), operators (+, -, *, /), clear (C), and equal (=).

### STEP 5
Create a new file Calculator.css in src/ and add the styling.

### STEP 6
Open src/App.js and modify it.

### STEP 7
Start the development server.
  npm start

### STEP 8
Open http://localhost:3000/ in the browser.

### STEP 9
Test the calculator by entering numbers and operations.

### STEP 10
Fix styling issues and refine content placement.

### STEP 11
Deploy the website.

### STEP 12
Upload to GitHub Pages for free hosting.

## PROGRAM
~~~

import { useState, useCallback } from "react";

const styles = `
  @import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Exo+2:wght@300;400;600;700&display=swap');

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: #0d0d0d;
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    font-family: 'Exo 2', sans-serif;
  }

  .calc-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 24px;
  }

  .calc-title {
    font-family: 'Exo 2', sans-serif;
    font-weight: 700;
    font-size: 13px;
    letter-spacing: 6px;
    color: #00e5ff;
    text-transform: uppercase;
    opacity: 0.7;
  }

  .calculator {
    background: linear-gradient(145deg, #1a1a2e, #16213e);
    border-radius: 24px;
    padding: 24px;
    width: 320px;
    box-shadow:
      0 0 0 1px rgba(0,229,255,0.12),
      0 0 40px rgba(0,229,255,0.06),
      0 30px 80px rgba(0,0,0,0.6),
      inset 0 1px 0 rgba(255,255,255,0.05);
    position: relative;
  }

  .calculator::before {
    content: '';
    position: absolute;
    top: -1px; left: 20px; right: 20px;
    height: 1px;
    background: linear-gradient(90deg, transparent, rgba(0,229,255,0.4), transparent);
  }

  /* Display */
  .display {
    background: #080c14;
    border-radius: 14px;
    padding: 16px 20px 12px;
    margin-bottom: 20px;
    border: 1px solid rgba(0,229,255,0.1);
    position: relative;
    overflow: hidden;
    min-height: 100px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }

  .display::after {
    content: '';
    position: absolute;
    inset: 0;
    background: repeating-linear-gradient(
      0deg,
      transparent,
      transparent 2px,
      rgba(0,229,255,0.015) 2px,
      rgba(0,229,255,0.015) 4px
    );
    pointer-events: none;
  }

  .display-expression {
    font-family: 'Share Tech Mono', monospace;
    font-size: 13px;
    color: rgba(0,229,255,0.4);
    min-height: 18px;
    text-align: right;
    letter-spacing: 1px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }

  .display-value {
    font-family: 'Share Tech Mono', monospace;
    font-size: 42px;
    color: #00e5ff;
    text-align: right;
    letter-spacing: 2px;
    text-shadow:
      0 0 20px rgba(0,229,255,0.5),
      0 0 40px rgba(0,229,255,0.2);
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    transition: font-size 0.1s;
    line-height: 1.1;
  }

  .display-value.long { font-size: 28px; }
  .display-value.very-long { font-size: 20px; }

  /* Button Grid */
  .btn-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
  }

  .btn {
    border: none;
    border-radius: 12px;
    font-family: 'Exo 2', sans-serif;
    font-size: 18px;
    font-weight: 600;
    cursor: pointer;
    padding: 0;
    height: 58px;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: transform 0.08s, box-shadow 0.08s, filter 0.08s;
    position: relative;
    outline: none;
    -webkit-tap-highlight-color: transparent;
    user-select: none;
  }

  .btn:active {
    transform: scale(0.93);
    filter: brightness(1.2);
  }

  /* Number buttons */
  .btn-num {
    background: linear-gradient(145deg, #1e2a3a, #172030);
    color: #c8d8f0;
    box-shadow:
      0 4px 8px rgba(0,0,0,0.4),
      inset 0 1px 0 rgba(255,255,255,0.06);
    border: 1px solid rgba(255,255,255,0.06);
    font-size: 20px;
  }

  .btn-num:hover {
    background: linear-gradient(145deg, #243244, #1d2840);
    color: #e8f0ff;
  }

  /* Zero spans 2 cols */
  .btn-zero { grid-column: span 2; }

  /* Operator buttons */
  .btn-op {
    background: linear-gradient(145deg, #0f2a3a, #0a1f30);
    color: #00e5ff;
    box-shadow:
      0 4px 8px rgba(0,0,0,0.4),
      0 0 12px rgba(0,229,255,0.08),
      inset 0 1px 0 rgba(0,229,255,0.1);
    border: 1px solid rgba(0,229,255,0.2);
    font-size: 22px;
  }

  .btn-op:hover {
    background: linear-gradient(145deg, #122f40, #0d2435);
    box-shadow:
      0 4px 12px rgba(0,0,0,0.4),
      0 0 20px rgba(0,229,255,0.15),
      inset 0 1px 0 rgba(0,229,255,0.15);
    color: #40efff;
  }

  /* Equal button */
  .btn-eq {
    background: linear-gradient(145deg, #00b4d8, #0077a8);
    color: #fff;
    box-shadow:
      0 4px 16px rgba(0,180,216,0.4),
      0 0 30px rgba(0,180,216,0.2),
      inset 0 1px 0 rgba(255,255,255,0.2);
    border: 1px solid rgba(0,229,255,0.3);
    font-size: 24px;
    font-weight: 700;
    text-shadow: 0 1px 4px rgba(0,0,0,0.3);
  }

  .btn-eq:hover {
    background: linear-gradient(145deg, #00c8f0, #0088bf);
    box-shadow:
      0 6px 20px rgba(0,200,240,0.5),
      0 0 40px rgba(0,200,240,0.25),
      inset 0 1px 0 rgba(255,255,255,0.25);
  }

  /* Clear button */
  .btn-clear {
    background: linear-gradient(145deg, #3a0a1a, #2a0512);
    color: #ff4d6d;
    box-shadow:
      0 4px 8px rgba(0,0,0,0.4),
      0 0 12px rgba(255,77,109,0.08),
      inset 0 1px 0 rgba(255,77,109,0.1);
    border: 1px solid rgba(255,77,109,0.2);
    font-size: 16px;
    font-weight: 700;
    letter-spacing: 1px;
  }

  .btn-clear:hover {
    background: linear-gradient(145deg, #450c20, #320616);
    box-shadow:
      0 4px 12px rgba(0,0,0,0.4),
      0 0 20px rgba(255,77,109,0.2),
      inset 0 1px 0 rgba(255,77,109,0.15);
    color: #ff6b87;
  }

  /* Backspace */
  .btn-back {
    background: linear-gradient(145deg, #2a1a0a, #1e1205);
    color: #ffb347;
    box-shadow:
      0 4px 8px rgba(0,0,0,0.4),
      inset 0 1px 0 rgba(255,179,71,0.08);
    border: 1px solid rgba(255,179,71,0.15);
    font-size: 20px;
  }

  .btn-back:hover {
    color: #ffc870;
    background: linear-gradient(145deg, #321f0c, #261606);
  }

  /* Active operator indicator */
  .btn-op.active {
    background: linear-gradient(145deg, #003d52, #002e3d);
    box-shadow:
      0 0 20px rgba(0,229,255,0.3),
      inset 0 0 10px rgba(0,229,255,0.1);
    color: #ffffff;
    border-color: rgba(0,229,255,0.5);
  }

  /* Error state */
  .display-value.error { color: #ff4d6d; font-size: 24px; text-shadow: 0 0 20px rgba(255,77,109,0.5); }

  .calc-footer {
    font-size: 11px;
    color: rgba(255,255,255,0.15);
    letter-spacing: 3px;
    text-transform: uppercase;
    font-family: 'Share Tech Mono', monospace;
  }
`;

export default function Calculator() {
  const [display, setDisplay] = useState("0");
  const [expression, setExpression] = useState("");
  const [prevValue, setPrevValue] = useState(null);
  const [operator, setOperator] = useState(null);
  const [waitingForOperand, setWaitingForOperand] = useState(false);
  const [isError, setIsError] = useState(false);

  const inputDigit = useCallback((digit) => {
    if (isError) return;
    if (waitingForOperand) {
      setDisplay(String(digit));
      setWaitingForOperand(false);
    } else {
      setDisplay(display === "0" ? String(digit) : display + digit);
    }
  }, [display, waitingForOperand, isError]);

  const inputDecimal = useCallback(() => {
    if (isError) return;
    if (waitingForOperand) {
      setDisplay("0.");
      setWaitingForOperand(false);
      return;
    }
    if (!display.includes(".")) {
      setDisplay(display + ".");
    }
  }, [display, waitingForOperand, isError]);

  const handleOperator = useCallback((op) => {
    if (isError) return;
    const current = parseFloat(display);

    if (prevValue !== null && !waitingForOperand) {
      const result = calculate(prevValue, current, operator);
      if (result === "Error") {
        setDisplay("Error"); setIsError(true); setExpression(""); setPrevValue(null); setOperator(null);
        return;
      }
      const resultStr = formatResult(result);
      setExpression(`${resultStr} ${op}`);
      setDisplay(resultStr);
      setPrevValue(result);
    } else {
      setExpression(`${display} ${op}`);
      setPrevValue(current);
    }
    setOperator(op);
    setWaitingForOperand(true);
  }, [display, prevValue, operator, waitingForOperand, isError]);

  const handleEquals = useCallback(() => {
    if (isError || operator === null || prevValue === null) return;
    const current = parseFloat(display);
    const result = calculate(prevValue, current, operator);
    if (result === "Error") {
      setDisplay("Error"); setIsError(true); setExpression(""); setPrevValue(null); setOperator(null);
      return;
    }
    const resultStr = formatResult(result);
    setExpression(`${expression} ${display} =`);
    setDisplay(resultStr);
    setPrevValue(null);
    setOperator(null);
    setWaitingForOperand(true);
  }, [display, prevValue, operator, waitingForOperand, expression, isError]);

  const handleClear = useCallback(() => {
    setDisplay("0");
    setExpression("");
    setPrevValue(null);
    setOperator(null);
    setWaitingForOperand(false);
    setIsError(false);
  }, []);

  const handleBackspace = useCallback(() => {
    if (isError) { handleClear(); return; }
    if (waitingForOperand) return;
    if (display.length > 1) {
      setDisplay(display.slice(0, -1));
    } else {
      setDisplay("0");
    }
  }, [display, waitingForOperand, isError, handleClear]);

  function calculate(a, b, op) {
    switch (op) {
      case "+": return a + b;
      case "−": return a - b;
      case "×": return a * b;
      case "÷": return b === 0 ? "Error" : a / b;
      default: return b;
    }
  }

  function formatResult(num) {
    if (typeof num !== "number") return String(num);
    if (Number.isInteger(num)) return String(num);
    const str = num.toPrecision(10).replace(/\.?0+$/, "");
    return str.length > 12 ? num.toExponential(4) : str;
  }

  const displayLen = display.length;
  const displayClass = displayLen > 12 ? "display-value very-long" : displayLen > 8 ? "display-value long" : "display-value";

  const buttons = [
    { label: "C",   cls: "btn btn-clear",          action: handleClear },
    { label: "⌫",   cls: "btn btn-back",            action: handleBackspace },
    { label: "%",   cls: "btn btn-op",              action: () => { if (!isError) { setDisplay(String(parseFloat(display) / 100)); setWaitingForOperand(false); } } },
    { label: "÷",   cls: `btn btn-op${operator === "÷" && waitingForOperand ? " active" : ""}`,  action: () => handleOperator("÷") },
    { label: "7",   cls: "btn btn-num",             action: () => inputDigit("7") },
    { label: "8",   cls: "btn btn-num",             action: () => inputDigit("8") },
    { label: "9",   cls: "btn btn-num",             action: () => inputDigit("9") },
    { label: "×",   cls: `btn btn-op${operator === "×" && waitingForOperand ? " active" : ""}`,  action: () => handleOperator("×") },
    { label: "4",   cls: "btn btn-num",             action: () => inputDigit("4") },
    { label: "5",   cls: "btn btn-num",             action: () => inputDigit("5") },
    { label: "6",   cls: "btn btn-num",             action: () => inputDigit("6") },
    { label: "−",   cls: `btn btn-op${operator === "−" && waitingForOperand ? " active" : ""}`,  action: () => handleOperator("−") },
    { label: "1",   cls: "btn btn-num",             action: () => inputDigit("1") },
    { label: "2",   cls: "btn btn-num",             action: () => inputDigit("2") },
    { label: "3",   cls: "btn btn-num",             action: () => inputDigit("3") },
    { label: "+",   cls: `btn btn-op${operator === "+" && waitingForOperand ? " active" : ""}`,  action: () => handleOperator("+") },
    { label: "0",   cls: "btn btn-num btn-zero",    action: () => inputDigit("0") },
    { label: ".",   cls: "btn btn-num",             action: inputDecimal },
    { label: "=",   cls: "btn btn-eq",              action: handleEquals },
  ];

  return (
    <>
      <style>{styles}</style>
      <div className="calc-wrapper">
        <div className="calc-title">Simple Calculator</div>
        <div className="calculator">
          <div className="display">
            <div className="display-expression">{expression || "\u00A0"}</div>
            <div className={isError ? "display-value error" : displayClass}>{display}</div>
          </div>
          <div className="btn-grid">
            {buttons.map((btn, i) => (
              <button key={i} className={btn.cls} onClick={btn.action}>
                {btn.label}
              </button>
            ))}
          </div>
        </div>
        <div className="calc-footer">React.js · Ex04</div>
      </div>
    </>
  );
}


~~~

## OUTPUT

<img width="1207" height="774" alt="image" src="https://github.com/user-attachments/assets/8758f815-7c65-467b-bccb-76d3a7c46c7f" />


## RESULT
The program for developing a simple calculator in React.js is executed successfully.
