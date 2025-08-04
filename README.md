<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Psychological Safety Deficit Calculator</title>
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700&display=swap" rel="stylesheet" />
  <style>
    body {
      font-family: 'Montserrat', sans-serif;
      margin: 0;
      background-color: transparent;
    }
    .container {
      display: flex;
      flex-direction: column;
      padding: 2rem;
      max-width: 1600px;
      margin: 0 auto;
      box-sizing: border-box;
    }
    @media (min-width: 1024px) {
      .container {
        flex-direction: row;
        justify-content: space-between;
      }
    }
    .calculator, .results-box {
      background-color: white;
      padding: 2rem;
      border-radius: 20px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
      box-sizing: border-box;
    }
    .calculator {
      flex: 1.3;
      margin-right: 2rem;
      border: 2px solid #f10178;
    }
    .results-box {
      flex: 1;
      background-color: #f10178;
      color: white;
      min-height: 300px;
      align-self: flex-start;
      display: none;
    }
    .results-box h2 {
      font-size: 22px;
    }
    label {
      font-weight: bold;
      display: block;
      margin-top: 1.5rem;
    }
    input[type="number"], select {
      width: 100%;
      padding: 0.6rem;
      font-size: 1rem;
      border-radius: 30px;
      border: 1px dashed #5b01fa;
      font-family: 'Montserrat', sans-serif;
      margin-top: 0.5rem;
    }
    button {
      margin-top: 1.5rem;
      width: 100%;
      padding: 1rem;
      font-size: 1.2rem;
      background-color: #f10178;
      color: white;
      border: none;
      border-radius: 30px;
      cursor: pointer;
      font-family: 'Montserrat', sans-serif;
    }
    .grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 1rem;
      margin-top: 1rem;
    }
    .pink-line {
      height: 2px;
      background-color: #ea0b82;
      width: 50%;
      margin: 1.5rem 0;
    }
    .results-line-item {
      display: flex;
      justify-content: space-between;
      margin: 0.15rem 0;
      font-size: 0.85rem;
    }
    .results-line-item.bold {
      font-size: 1rem;
      font-weight: bold;
    }
    .total-line {
      font-size: 1.2rem;
      font-weight: bold;
      margin: 2rem 0 1rem 0;
      display: flex;
      justify-content: space-between;
      border-top: 1px dotted white;
      border-bottom: 1px dotted white;
      padding: 0.5rem 0;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="calculator">
      <h1>Psychological Safety Deficit Calculator</h1>

      <h2>Total Staff Complement</h2>
      <input type="number" id="totalStaff" placeholder="e.g. 1000" />
      <div class="pink-line"></div>

      <div class="grid">
        <div>
          <label>% Black</label>
          <input type="number" id="blackPct" />
        </div>
        <div>
          <label>% White</label>
          <input type="number" id="whitePct" />
        </div>
        <div>
          <label>% Coloured</label>
          <input type="number" id="colouredPct" />
        </div>
        <div>
          <label>% Indian/Asian</label>
          <input type="number" id="indianasianPct" />
        </div>
      </div>

      <div class="pink-line"></div>

      <div class="grid">
        <div>
          <label>% Women</label>
          <input type="number" id="womenPct" />
        </div>
        <div>
          <label>% Men</label>
          <input type="number" id="menPct" />
        </div>
      </div>

      <h2>Average Annual Salary per Race Group (ZAR)</h2>
      <div class="grid">
        <div>
          <label>Black</label>
          <input type="number" id="blackSalary" />
        </div>
        <div>
          <label>White</label>
          <input type="number" id="whiteSalary" />
        </div>
        <div>
          <label>Coloured</label>
          <input type="number" id="colouredSalary" />
        </div>
        <div>
          <label>Indian/Asian</label>
          <input type="number" id="indianasianSalary" />
        </div>
      </div>

      <h2>How would you rate your organisation's culture of Inclusivity & Psychological Safety?</h2>
      <select id="cultureRating">
        <option value="" disabled selected hidden>Select</option>
        <option value="low">Low</option>
        <option value="medium">Medium</option>
        <option value="high">High</option>
      </select>

      <button onclick="calculateCosts()">Calculate</button>
    </div>

    <div class="results-box" id="result">
      <!-- Results injected by JS -->
    </div>
  </div>

  <script>
    function getRates(level) {
      const rates = {
        low: {
          turnoverRates: {
            black: { men: 0.07, women: 0.08 },
            white: { men: 0.01, women: 0.015 },
            coloured: { men: 0.03, women: 0.04 },
            indianasian: { men: 0.03, women: 0.04 }
          },
          absenteeismDays: {
            black: 2,
            white: 0.5,
            coloured: 1,
            indianasian: 1
          },
          presenteeismRates: {
            black: 0.15,
            white: 0.0375,
            coloured: 0.09375,
            indianasian: 0.09375
          }
        },
        medium: {
          turnoverRates: {
            black: { men: 0.03, women: 0.04 },
            white: { men: 0.005, women: 0.01 },
            coloured: { men: 0.01, women: 0.02 },
            indianasian: { men: 0.01, women: 0.02 }
          },
          absenteeismDays: {
            black: 1,
            white: 0.25,
            coloured: 0.5,
            indianasian: 0.5
          },
          presenteeismRates: {
            black: 0.075,
            white: 0.015,
            coloured: 0.045,
            indianasian: 0.045
          }
        },
        high: {
          turnoverRates: {
            black: { men: 0.01, women: 0.02 },
            white: { men: 0.0025, women: 0.005 },
            coloured: { men: 0.005, women: 0.01 },
            indianasian: { men: 0.005, women: 0.01 }
          },
          absenteeismDays: {
            black: 0.5,
            white: 0.1,
            coloured: 0.25,
            indianasian: 0.25
          },
          presenteeismRates: {
            black: 0.0375,
            white: 0.0075,
            coloured: 0.01875,
            indianasian: 0.01875
          }
        }
      };
      return rates[level];
    }

    function calculateCosts() {
      const total = parseFloat(document.getElementById('totalStaff').value);
      const getPct = id => parseFloat(document.getElementById(id).value || 0) / 100;
      const getVal = id => parseFloat(document.getElementById(id).value || 0);

      const genderSplit = {
        men: getPct('menPct'),
        women: getPct('womenPct')
      };

      const culture = document.getElementById('cultureRating').value;
      if (!culture) {
        alert("Please select a culture rating.");
        return;
      }

      const { turnoverRates, absenteeismDays, presenteeismRates } = getRates(culture);

      const raceGroups = {
        black: {
          pct: getPct('blackPct'),
          salary: getVal('blackSalary')
        },
        white: {
          pct: getPct('whitePct'),
          salary: getVal('whiteSalary')
        },
        coloured: {
          pct: getPct('colouredPct'),
          salary: getVal('colouredSalary')
        },
        indianasian: {
          pct: getPct('indianasianPct'),
          salary: getVal('indianasianSalary')
        }
      };

      let turnoverCost = 0;
      let absenteeismCost = 0;
      let presenteeismCost = 0;
      let totalExits = 0;

      for (const [race, group] of Object.entries(raceGroups)) {
        const headcount = total * group.pct;
        const maleHeadcount = headcount * genderSplit.men;
        const femaleHeadcount = headcount * genderSplit.women;

        const exits = (maleHeadcount * turnoverRates[race].men) + (femaleHeadcount * turnoverRates[race].women);
        totalExits += exits;
        turnoverCost += exits * (0.5 * group.salary);

        absenteeismCost += absenteeismDays[race] * (group.salary / 220) * headcount * 0.88;
        presenteeismCost += headcount * group.salary * presenteeismRates[race];
      }

      const totalCost = turnoverCost + absenteeismCost + presenteeismCost;

      const resultDiv = document.getElementById('result');
      resultDiv.style.display = 'block';
      resultDiv.innerHTML = `
        <h2>Estimated Annual Cost of DEI Neglect</h2>
        <div class="results-line-item"><span><strong>EXCESS Resignations:</strong></span><span>${totalExits.toFixed(1)}</span></div>
        <div class="results-line-item"><span>Turnover:</span><span>R ${Math.round(turnoverCost).toLocaleString()}</span></div>
        <div class="results-line-item"><span>Absenteeism:</span><span>R ${Math.round(absenteeismCost).toLocaleString()}</span></div>
        <div class="results-line-item"><span>Presenteeism:</span><span>R ${Math.round(presenteeismCost).toLocaleString()}</span></div>
        <div class="total-line"><span>Total:</span><span>R ${Math.round(totalCost).toLocaleString()}</span></div>
      `;
    }
  </script>
</body>
</html>
