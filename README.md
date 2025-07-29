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
      max-width: 600px;
      margin: 0;
      padding: 2rem;
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
      border: none;
    }
    .title-underline {
      display: none;
    }
    h2 {
      font-size: 0.35rem;
      font-weight: 700;
      margin-top: 1rem;
      margin-bottom: 0.5rem;
      border: none;
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
    .purple-card input#totalStaff,
    .purple-card input#womenPct,
    .purple-card input#menPct,
    .purple-card input#blackPct,
    .purple-card input#whitePct,
    .purple-card input#colouredPct,
    .purple-card input#indianasianPct,
    .purple-card select#cultureRating {
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
</body>
</html>
