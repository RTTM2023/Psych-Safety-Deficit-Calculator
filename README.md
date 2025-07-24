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
    }
    @media (min-width: 1024px) {
      .container {
        flex-direction: row;
        justify-content: space-between;
        align-items: flex-start;
      }
    }
    .calculator, .results-box {
      background-color: white;
      padding: 2rem;
      border-radius: 20px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
      margin-bottom: 2rem;
    }
    @media (min-width: 1024px) {
      .calculator {
        flex: 1.3;
        margin-right: 2rem;
      }
      .results-box {
        flex: 1;
        margin-bottom: 0;
      }
    }
    .calculator {
      border: 2px solid #f10178;
    }
    .results-box {
      background-color: #f10178;
      color: white;
      height: auto;
      min-height: 300px;
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
    }
    button {
      margin-top: 1rem;
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
    .results-line-item {
      display: flex;
      justify-content: space-between;
      margin: 0.15rem 0;
      font-size: 0.75rem;
    }
    .results-line-item.bold {
      font-size: 0.9rem;
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

    <div class="section">
      <label for="totalStaff">Total Staff Complement</label>
      <input type="number" id="totalStaff" placeholder="e.g. 1000" />
    </div>

    <div class="section">
      <div class="row">
        <div class="column">
          <label for="black">% Black Employees</label>
          <input type="number" id="black" placeholder="e.g. 70" />
        </div>
        <div class="column">
          <label for="white">% White Employees</label>
          <input type="number" id="white" placeholder="e.g. 20" />
        </div>
        <div class="column">
          <label for="coloured">% Coloured Employees</label>
          <input type="number" id="coloured" placeholder="e.g. 5" />
        </div>
        <div class="column">
          <label for="indian">% Indian/Asian Employees</label>
          <input type="number" id="indian" placeholder="e.g. 5" />
        </div>
      </div>
    </div>

    <div class="section">
      <div class="row">
        <div class="column">
          <label for="women">% Women</label>
          <input type="number" id="women" placeholder="e.g. 40" />
        </div>
        <div class="column">
          <label for="men">% Men</label>
          <input type="number" id="men" placeholder="e.g. 60" />
        </div>
      </div>
    </div>

    <div class="section">
      <div class="row">
        <div class="column">
          <label for="salaryBlack">Avg Annual Salary (Black)</label>
          <input type="number" id="salaryBlack" placeholder="e.g. 300000" />
        </div>
        <div class="column">
          <label for="salaryWhite">Avg Annual Salary (White)</label>
          <input type="number" id="salaryWhite" placeholder="e.g. 500000" />
        </div>
        <div class="column">
          <label for="salaryColoured">Avg Annual Salary (Coloured)</label>
          <input type="number" id="salaryColoured" placeholder="e.g. 350000" />
        </div>
        <div class="column">
          <label for="salaryIndian">Avg Annual Salary (Indian/Asian)</label>
          <input type="number" id="salaryIndian" placeholder="e.g. 450000" />
        </div>
      </div>
    </div>

    <div class="section">
      <label for="culture">How would you rate your current culture?</label>
      <select id="culture">
        <option value="Low">Low</option>
        <option value="Medium">Medium</option>
        <option value="High">High</option>
      </select>

      <label for="benefit">Expected Improvement from Programme</label>
      <select id="benefit">
        <option value="0.2">Mild (20%)</option>
        <option value="0.3">Moderate (30%)</option>
        <option value="0.4">High (40%)</option>
      </select>
    </div>

    <button onclick="calculateCostSaving()">Calculate</button>

    <div class="results" id="results" style="display:none"></div>
  </div>

  <script>
    const turnoverRates = {
      Low: {
        "Black Women": 0.08, "Black Men": 0.07,
        "White Women": 0.015, "White Men": 0.01,
        "Coloured Women": 0.04, "Coloured Men": 0.03,
        "Indian/Asian Women": 0.04, "Indian/Asian Men": 0.03
      },
      Medium: {
        "Black Women": 0.04, "Black Men": 0.03,
        "White Women": 0.01, "White Men": 0.005,
        "Coloured Women": 0.02, "Coloured Men": 0.01,
        "Indian/Asian Women": 0.02, "Indian/Asian Men": 0.01
      },
      High: {
        "Black Women": 0.02, "Black Men": 0.01,
        "White Women": 0.005, "White Men": 0.0025,
        "Coloured Women": 0.01, "Coloured Men": 0.005,
        "Indian/Asian Women": 0.01, "Indian/Asian Men": 0.005
      }
    };

    const absenteeismRates = {
      Low: { Black: 2, White: 0.5, Coloured: 1, "Indian/Asian": 1 },
      Medium: { Black: 1, White: 0.25, Coloured: 0.5, "Indian/Asian": 0.5 },
      High: { Black: 0.5, White: 0.1, Coloured: 0.25, "Indian/Asian": 0.25 }
    };

    function calculateCostSaving() {
      const staff = parseInt(document.getElementById('totalStaff').value);
      const culture = document.getElementById('culture').value;
      const benefit = parseFloat(document.getElementById('benefit').value);

      const percentages = {
        Black: parseFloat(document.getElementById('black').value) || 0,
        White: parseFloat(document.getElementById('white').value) || 0,
        Coloured: parseFloat(document.getElementById('coloured').value) || 0,
        Indian: parseFloat(document.getElementById('indian').value) || 0
      };

      const gender = {
        Women: parseFloat(document.getElementById('women').value) || 0,
        Men: parseFloat(document.getElementById('men').value) || 0
      };

      const salaries = {
        Black: parseFloat(document.getElementById('salaryBlack').value) || 0,
        White: parseFloat(document.getElementById('salaryWhite').value) || 0,
        Coloured: parseFloat(document.getElementById('salaryColoured').value) || 0,
        Indian: parseFloat(document.getElementById('salaryIndian').value) || 0
      };

      let turnover = 0;
      let absenteeism = 0;

      function calcTurnover(race, salary, pct) {
        const femaleRate = turnoverRates[culture][`${race} Women`] || 0;
        const maleRate = turnoverRates[culture][`${race} Men`] || 0;
        const racePct = pct / 100;
        const womenPct = gender.Women / 100;
        const menPct = gender.Men / 100;
        const femaleHeadcount = staff * racePct * womenPct;
        const maleHeadcount = staff * racePct * menPct;
        return ((femaleHeadcount * salary * 0.5 * femaleRate) + (maleHeadcount * salary * 0.5 * maleRate)) * benefit;
      }

      function calcAbsenteeism(race, salary, pct) {
        const days = absenteeismRates[culture][race] || 0;
        const dailyRate = salary / 260;
        return (staff * (pct / 100) * days * dailyRate) * benefit;
      }

      turnover += calcTurnover("Black", salaries.Black, percentages.Black);
      turnover += calcTurnover("White", salaries.White, percentages.White);
      turnover += calcTurnover("Coloured", salaries.Coloured, percentages.Coloured);
      turnover += calcTurnover("Indian/Asian", salaries.Indian, percentages.Indian);

      absenteeism += calcAbsenteeism("Black", salaries.Black, percentages.Black);
      absenteeism += calcAbsenteeism("White", salaries.White, percentages.White);
      absenteeism += calcAbsenteeism("Coloured", salaries.Coloured, percentages.Coloured);
      absenteeism += calcAbsenteeism("Indian/Asian", salaries.Indian, percentages.Indian);

      const total = turnover + absenteeism;

      document.getElementById('results').style.display = 'block';
      document.getElementById('results').innerHTML = `
        <strong>Estimated Annual Cost Saving:</strong><br/>
        Turnover Cost Saving: R ${Math.round(turnover).toLocaleString()}<br/>
        Absenteeism Cost Saving: R ${Math.round(absenteeism).toLocaleString()}<br/>
        <hr>
        <strong>Total Cost Saving: R ${Math.round(total).toLocaleString()}</strong>
      `;
    }
  </script>
</body>
</html>
