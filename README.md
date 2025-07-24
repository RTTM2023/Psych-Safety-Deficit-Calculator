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
      background-color: #f9f9f9;
    }
    .container {
      display: flex;
      flex-direction: column;
      padding: 2rem;
      max-width: 1200px;
      margin: 0 auto;
    }
    h1 {
      text-align: center;
      font-weight: 700;
      margin-bottom: 1.5rem;
    }
    .input-group {
      margin-bottom: 1rem;
    }
    .input-group label {
      display: block;
      font-weight: 600;
      margin-bottom: 0.3rem;
    }
    .input-group input, .input-group select {
      padding: 0.5rem;
      width: 100%;
      font-size: 1rem;
      border: 1px solid #ccc;
      border-radius: 4px;
    }
    button {
      padding: 0.75rem 1.5rem;
      background-color: #0044cc;
      color: #fff;
      border: none;
      border-radius: 4px;
      font-size: 1rem;
      cursor: pointer;
    }
    .results {
      margin-top: 2rem;
      padding: 1rem;
      background-color: #e6f7ff;
      border-left: 5px solid #1890ff;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Psychological Safety Deficit Calculator</h1>

    <div class="input-group">
      <label for="totalStaff">Total Staff Complement</label>
      <input type="number" id="totalStaff" placeholder="e.g. 1000" />
    </div>

    <div class="input-group">
      <label for="culture">How would you rate your current culture?</label>
      <select id="culture">
        <option value="Low">Low</option>
        <option value="Medium">Medium</option>
        <option value="High">High</option>
      </select>
    </div>

    <div class="input-group">
      <label for="benefit">Expected Improvement from Programme</label>
      <select id="benefit">
        <option value="0.2">Mild (20%)</option>
        <option value="0.3">Moderate (30%)</option>
        <option value="0.4">High (40%)</option>
      </select>
    </div>

    <!-- Add more input groups for demographics and salary breakdowns as needed -->

    <button onclick="calculateCostSaving()">Calculate</button>

    <div class="results" id="results" style="display:none"></div>
  </div>

  <script>
    const turnoverRates = {
      Low: {
        "Black Women": 0.08, "Black Men": 0.07,
        "White Women": 0.015, "White Men": 0.01,
        "Colored Women": 0.04, "Colored Men": 0.03,
        "Indian/Asian Women": 0.04, "Indian/Asian Men": 0.03
      },
      Medium: {
        "Black Women": 0.04, "Black Men": 0.03,
        "White Women": 0.01, "White Men": 0.005,
        "Colored Women": 0.02, "Colored Men": 0.01,
        "Indian/Asian Women": 0.02, "Indian/Asian Men": 0.01
      },
      High: {
        "Black Women": 0.02, "Black Men": 0.01,
        "White Women": 0.005, "White Men": 0.0025,
        "Colored Women": 0.01, "Colored Men": 0.005,
        "Indian/Asian Women": 0.01, "Indian/Asian Men": 0.005
      }
    };

    const absenteeismRates = {
      Low: { Black: 2, White: 0.5, Colored: 1, "Indian/Asian": 1 },
      Medium: { Black: 1, White: 0.25, Colored: 0.5, "Indian/Asian": 0.5 },
      High: { Black: 0.5, White: 0.1, Colored: 0.25, "Indian/Asian": 0.25 }
    };

    function calculateCostSaving() {
      const staff = parseInt(document.getElementById('totalStaff').value);
      const culture = document.getElementById('culture').value;
      const benefit = parseFloat(document.getElementById('benefit').value);

      if (!staff || staff <= 0) {
        alert("Please enter a valid staff number.");
        return;
      }

      // Placeholder logic – replace with real calculation
      const turnoverCost = staff * 0.1 * 500000 * benefit; // 10% turnover
      const absenteeismCost = staff * 1 * (500000 / 260) * benefit; // 1 sick day

      const total = turnoverCost + absenteeismCost;

      document.getElementById('results').style.display = 'block';
      document.getElementById('results').innerHTML = `
        <strong>Estimated Annual Cost Saving:</strong><br/>
        Turnover Cost Saving: R ${turnoverCost.toLocaleString()}<br/>
        Absenteeism Cost Saving: R ${absenteeismCost.toLocaleString()}<br/>
        <hr>
        <strong>Total Cost Saving: R ${total.toLocaleString()}</strong>
      `;
    }
  </script>
</body>
</html>
