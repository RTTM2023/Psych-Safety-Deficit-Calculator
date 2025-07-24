<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>DEI Neglect Cost Calculator</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 2rem;
      max-width: 900px;
      margin: auto;
    }
    h1, h2 {
      color: #333;
    }
    label {
      display: block;
      margin-top: 1rem;
    }
    input, select {
      padding: 0.5rem;
      width: 100%;
      box-sizing: border-box;
    }
    button {
      margin-top: 1.5rem;
      padding: 0.75rem 1.5rem;
      background-color: #007bff;
      color: #fff;
      border: none;
      cursor: pointer;
    }
    .result {
      margin-top: 2rem;
      padding: 1rem;
      border: 1px solid #ccc;
      background-color: #f9f9f9;
    }
  </style>
</head>
<body>
  <h1>DEI Neglect Cost Calculator</h1>

  <label>Total Staff Complement:</label>
  <input type="number" id="totalStaff" placeholder="e.g. 1000" />

  <h2>Workforce Breakdown by Race (%)</h2>
  <label>Black:</label>
  <input type="number" id="blackPct" />
  <label>White:</label>
  <input type="number" id="whitePct" />
  <label>Coloured:</label>
  <input type="number" id="colouredPct" />
  <label>Indian/Asian:</label>
  <input type="number" id="indianPct" />

  <h2>Workforce Breakdown by Gender (%)</h2>
  <label>Women:</label>
  <input type="number" id="womenPct" />
  <label>Men:</label>
  <input type="number" id="menPct" />

  <h2>Average Annual Salary (across all groups)</h2>
  <input type="number" id="avgSalary" placeholder="e.g. 250000" />

  <h2>How would you rate your organisation's culture of Inclusivity & Psychological Safety?</h2>
  <select id="rating">
    <option value="low">Poor</option>
    <option value="medium" selected>Moderate</option>
    <option value="high">Strong</option>
  </select>

  <button onclick="calculateCosts()">Calculate Costs</button>

  <div class="result" id="result"></div>

  <script>
    function calculateCosts() {
      const total = parseFloat(document.getElementById('totalStaff').value);
      const getPct = id => parseFloat(document.getElementById(id).value || 0) / 100;
      const avgSalary = parseFloat(document.getElementById('avgSalary').value || 0);

      const raceGroups = {
        black: { pct: getPct('blackPct'), turnoverRate: 0.04 },
        white: { pct: getPct('whitePct'), turnoverRate: 0.015 },
        coloured: { pct: getPct('colouredPct'), turnoverRate: 0.045 },
        indian: { pct: getPct('indianPct'), turnoverRate: 0.045 }
      };

      const absenteeismDays = 3;
      const presenteeismRate = 0.03;
      const rating = document.getElementById('rating').value;
      const reductionFactor = rating === 'high' ? 0.6 : rating === 'medium' ? 0.8 : 1.0;

      let turnoverCost = 0;
      let absenteeismCost = 0;
      let presenteeismCost = 0;

      for (const group of Object.values(raceGroups)) {
        const headcount = total * group.pct;
        const exits = headcount * group.turnoverRate;
        turnoverCost += exits * (1.5 * avgSalary);
        absenteeismCost += headcount * absenteeismDays * (avgSalary / 260);
        presenteeismCost += headcount * avgSalary * presenteeismRate;
      }

      turnoverCost *= reductionFactor;
      absenteeismCost *= reductionFactor;
      presenteeismCost *= reductionFactor;

      const totalCost = turnoverCost + absenteeismCost + presenteeismCost;

      document.getElementById('result').innerHTML = `
        <h2>Estimated Annual Cost of DEI Neglect</h2>
        <p><strong>Turnover:</strong> R ${turnoverCost.toLocaleString()}</p>
        <p><strong>Absenteeism:</strong> R ${absenteeismCost.toLocaleString()}</p>
        <p><strong>Presenteeism:</strong> R ${presenteeismCost.toLocaleString()}</p>
        <p><strong>Total:</strong> <strong>R ${totalCost.toLocaleString()}</strong></p>
      `;
    }
  </script>
</body>
</html>
