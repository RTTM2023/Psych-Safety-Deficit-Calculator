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
      background-color: #ffffff;
    }
    .container {
      display: flex;
      flex-direction: column;
      padding: 2rem;
      max-width: 1600px;
      margin: 0 auto;
    }
    h1 {
      font-size: 2rem;
      font-weight: 700;
      margin-bottom: 1.5rem;
    }
    .section {
      margin-bottom: 2rem;
    }
    label {
      font-weight: 600;
      display: block;
      margin-bottom: 0.3rem;
    }
    input, select {
      width: 100%;
      padding: 0.5rem;
      font-size: 1rem;
      border: 1px solid #ccc;
      border-radius: 4px;
      margin-bottom: 1rem;
    }
    .row {
      display: flex;
      gap: 1rem;
      flex-wrap: wrap;
    }
    .column {
      flex: 1;
      min-width: 200px;
    }
    button {
      padding: 0.75rem 1.5rem;
      background-color: #0044cc;
      color: #fff;
      border: none;
      border-radius: 4px;
      font-size: 1rem;
      cursor: pointer;
      width: 200px;
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
    function calculateCostSaving() {
      // To be filled in next step: logic using all inputs
      const total = parseInt(document.getElementById('totalStaff').value);
      if (!total || total <= 0) {
        alert('Please enter a valid total staff number.');
        return;
      }
      document.getElementById('results').style.display = 'block';
      document.getElementById('results').innerHTML = `<strong>Calculation logic still in progress</strong>`;
    }
  </script>
</body>
</html>
