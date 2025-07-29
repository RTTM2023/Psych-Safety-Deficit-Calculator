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
    .container {
      max-width: 768px;
      margin: 0 auto;
      padding: 2rem;
    }
    .card {
      background-color: #e4c8f7;
      border-radius: 24px;
      padding: 2rem;
      margin-bottom: 2rem;
    }
    .subcard {
      background-color: white;
      border: 1px solid #ccc;
      border-radius: 24px;
      padding: 2rem;
    }
    h1 {
      font-size: 1.75rem;
      font-weight: 700;
      margin-bottom: 1rem;
    }
    h2 {
      font-size: 1rem;
      font-weight: 700;
      margin-top: 2rem;
      margin-bottom: 0.5rem;
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
      margin-bottom: 1rem;
      border: none;
      border-radius: 16px;
      background-color: #e4c8f7;
      font-family: 'Montserrat', sans-serif;
    }
    .grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 1rem;
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
      margin-top: 1rem;
      cursor: pointer;
    }
    .result {
      background-color: white;
      border-radius: 16px;
      padding: 1rem;
      margin-top: 2rem;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="card">
      <h1>Psychological Safety Deficit Calculator</h1>
      <label>Total Staff Complement</label>
      <input type="number" id="totalStaff" placeholder="e.g. 1000" />

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

    <div class="subcard">
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

    <div class="card">
      <label><strong>How would you rate your organisation's culture of Inclusivity & Psychological Safety?</strong></label>
      <select id="cultureRating">
        <option value="low">Low</option>
        <option value="medium" selected>Medium</option>
        <option value="high">High</option>
      </select>
    </div>

    <button onclick="calculateCosts()">Calculate</button>
    <div class="result" id="result"></div>
  </div>

  <!-- SCRIPT OMITTED FOR BREVITY, STILL ACTIVE IN PAGE -->
</body>
</html>
