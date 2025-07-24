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
      max-width: 800px;
      margin: auto;
    }
    h1, h2 {
      color: #333;
    }
    label {
      display: block;
      margin-top: 1rem;
    }
    input {
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

  <label>% Black Women:</label>
  <input type="number" id="blackWomen" placeholder="e.g. 40" />

  <label>% Black Men:</label>
  <input type="number" id="blackMen" placeholder="e.g. 30" />

  <label>% White Men:</label>
  <input type="number" id="whiteMen" placeholder="e.g. 15" />

  <label>% Coloured:</label>
  <input type="number" id="coloured" placeholder="e.g. 10" />

  <label>% Indian/Asian:</label>
  <input type="number" id="indianAsian" placeholder="e.g. 5" />

  <button onclick="calculateCosts()">Calculate Costs</button>

  <div class="result" id="result"></div>

  <script>
    function calculateCosts() {
      const total = parseFloat(document.getElementById('totalStaff').value);
      const pct = key => parseFloat(document.getElementById(key).value || 0) / 100;

      const groups = {
        blackWomen: { rate: 0.04 },
        blackMen: { rate: 0.03 },
        whiteMen: { rate: 0.005 },
        coloured: { rate: 0.045 },
        indianAsian: { rate: 0.045 }
      };

      let turnoverCost = 0;
      for (const [key, { rate }] of Object.entries(groups)) {
        const headcount = total * pct(key);
        const exits = headcount * rate;
        const costPerExit = 1.5 * 250000; // assume R250k avg salary * 1.5 replacement cost
        turnoverCost += exits * costPerExit;
      }

      const absenteeismCost = total * 3 * (250000 / 260 / 8); // 3 days lost * hourly rate
      const presenteeismCost = total * 250000 * 0.03; // 3% underperformance waste
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
