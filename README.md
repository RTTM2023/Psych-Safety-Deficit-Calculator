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
      background-color: #f7f1fc;
    }
    .main-wrapper {
      display: flex;
      gap: 2rem;
      align-items: flex-start;
      padding: 2rem;
    }
    .container {
      width: 600px;
      padding: 0;
      flex-shrink: 0;
    }
    .card, .subcard {
      border-radius: 24px;
      padding: 2rem;
      margin-bottom: 0;
      border: 1px solid #E3C8F7;
    }
    .card {
      background-color: white;
    }
    .purple-card {
      background-color: #e4c8f7;
    }
    h1 {
      font-size: 1.75rem;
      font-weight: 700;
      margin-bottom: 0.5rem;
    }
    h2 {
      font-size: 2rem;
      font-weight: 700;
      margin: 1.5rem 0 1rem 0;
    }
    label {
      font-weight: 500;
      font-size: 0.9rem;
      display: block;
      margin-bottom: 0.25rem;
    }
    input, select {
      width: 100%;
      padding: 0.75rem;
      margin-bottom: 0;
      border: none;
      border-radius: 30px;
      font-family: 'Montserrat', sans-serif;
    }
    .purple-card input,
    .purple-card select {
      background-color: white;
    }
    .card input, .card select {
      background-color: #E3C8F7;
    }
    .grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 1rem;
    }
    .pink-line {
      height: 2px;
      background-color: #ea0b82;
      width: 90%;
      margin: 1.5rem 0;
    }
    button {
      width: 100%;
      background-color: #5200ff;
      color: white;
      font-weight: 700;
      padding: 1rem;
      border: none;
      border-radius: 20px;
      font-size: 1rem;
      margin: 0;
      cursor: pointer;
    }
    .result-box {
      background-color: #5700ff;
      color: white;
      border-radius: 24px;
      padding: 1.5rem;
      max-width: 400px;
      display: none;
    }
    .result-box h2 {
      font-size: 1.2rem;
      margin-bottom: 1rem;
      text-align: left;
    }
    .result-item {
      display: flex;
      justify-content: space-between;
      margin-bottom: 0.5rem;
    }
    .total-cost {
      font-size: 1.55rem;
      font-weight: bold;
      border-top: 2px dotted #ccc;
      border-bottom: 2px dotted #ccc;
      padding: 1rem 0;
      display: flex;
      justify-content: space-between;
      margin-top: 1rem;
    }
    .button-group {
      margin-top: 1rem;
      display: flex;
      flex-direction: column;
      gap: 0.5rem;
    }
    .button-group button {
      width: 100%;
      font-weight: 700;
      padding: 1rem;
      border: none;
      border-radius: 20px;
      font-size: 1rem;
      cursor: pointer;
    }
    .reset {
      background-color: #ea0b82;
      color: white;
    }
    .download {
      background-color: white;
      color: #5200ff;
    }
    .enquire {
      background-color: white;
      color: #5200ff;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="card">
      <h1>Psychological Safety Deficit Calculator</h1>
    </div>

    <!-- Section 1 -->
    <div class="card purple-card">
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
    </div>

    <!-- Section 2 -->
    <div class="card">
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
    </div>

    <!-- Section 3 -->
    <div class="card purple-card">
      <h2>How would you rate your organisation's culture of Inclusivity & Psychological Safety?</h2>
      <select id="cultureRating">
        <option value="" disabled selected hidden>Select</option>
        <option value="low">Low</option>
        <option value="medium">Medium</option>
        <option value="high">High</option>
      </select>
    </div>

    <button onclick="calculateCosts()">Calculate</button>
    <div class="result" id="result"></div>
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
        <p><strong>EXCESS Resignations:</strong> ${totalExits.toFixed(1)}</p>
        <p><strong>Turnover:</strong> R ${Math.round(turnoverCost).toLocaleString()}</p>
        <p><strong>Absenteeism:</strong> R ${Math.round(absenteeismCost).toLocaleString()}</p>
        <p><strong>Presenteeism:</strong> R ${Math.round(presenteeismCost).toLocaleString()}</p>
        <p><strong>Total:</strong> <strong>R ${Math.round(totalCost).toLocaleString()}</strong></p>
      `;
    }
  </script>
</body>
</html>
