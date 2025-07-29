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
      max-width: 520px;
      margin: 0 auto;
      padding: 2rem;
    }
    .card, .subcard {
      border-radius: 24px;
      padding: 2rem;
      margin-bottom: 1rem;
    }
    .card {
      background-color: white;
      border: 1px solid #ccc;
    }
    .purple-card {
      background-color: #e4c8f7;
    }
    h1 {
      font-size: 1.75rem;
      font-weight: 700;
      margin-bottom: 0.5rem;
    }
    .title-underline {
      height: 2px;
      background-color: #5200ff;
      width: 75%;
      margin: 0.5rem 0 0 0;
    }
    h2 {
      font-size: 0.85rem;
      font-weight: 700;
      margin-top: 1rem;
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
      border-radius: 30px;
      background-color: #e4c8f7;
      font-family: 'Montserrat', sans-serif;
    }
    .card input, .card select {
      background-color: white;
    }
    .grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 1rem;
    }
    .pink-line {
      height: 2px;
      background-color: #ea0b82;
      width: 50%;
      margin-bottom: 1rem;
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
      <div class="title-underline"></div>
    </div>

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

    <div class="card">
      <h2>Average Annual Salary per Race Group (ZAR)</h2>
      <div class="grid">
        <div>
          <label>Black</label>
          <input type="number" id="blackSalary" class="purple-card" />
        </div>
        <div>
          <label>White</label>
          <input type="number" id="whiteSalary" class="purple-card" />
        </div>
        <div>
          <label>Coloured</label>
          <input type="number" id="colouredSalary" class="purple-card" />
        </div>
        <div>
          <label>Indian/Asian</label>
          <input type="number" id="indianasianSalary" class="purple-card" />
        </div>
      </div>
    </div>

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
</body>
</html>
