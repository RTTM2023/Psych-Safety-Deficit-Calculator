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
        gap: 2rem;
      }
    }
    .calculator {
      flex: 1.8;
      background-color: white;
      padding: 2rem;
      border-radius: 20px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
      border: 2px solid #f10178;
    }
    .results-box {
      flex: 1.2;
      background-color: #f10178;
      color: white;
      padding: 2rem;
      border-radius: 20px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
      min-height: 300px;
    }
    .results-box h2 {
      font-size: 22px;
    }
    label {
      font-weight: bold;
      display: block;
      margin-top: 1.5rem;
      word-wrap: break-word;
    }
    input[type="number"], select, textarea {
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
    .input-row {
      display: flex;
      flex-wrap: wrap;
      gap: 1rem;
    }
    .input-group {
      flex: 1 1 220px;
      min-width: 220px;
    }
    .button-group {
      display: flex;
      flex-direction: column;
      gap: 1rem;
      margin-top: 2rem;
    }
    .reset-btn, .download-btn, .enquiry-btn {
      background: white;
      color: #f10178;
      border: 1px dashed #5b01fa;
      font-size: 1rem;
      font-weight: 500;
      font-family: 'Montserrat', sans-serif;
      padding: 0.75rem 1rem;
      border-radius: 30px;
      cursor: pointer;
      text-align: center;
    }
    .reset-btn {
      background: #5b01fa;
      color: white;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="calculator">
      <h1>Psychological Safety Deficit Calculator</h1>

      <div class="input-group">
        <label for="totalStaff">Total Staff Complement</label>
        <input type="number" id="totalStaff" placeholder="e.g. 1000" />
      </div>

      <div class="input-row">
        <div class="input-group">
          <label for="black">% Black Employees</label>
          <input type="number" id="black" placeholder="e.g. 70" />
        </div>
        <div class="input-group">
          <label for="white">% White Employees</label>
          <input type="number" id="white" placeholder="e.g. 20" />
        </div>
        <div class="input-group">
          <label for="coloured">% Coloured Employees</label>
          <input type="number" id="coloured" placeholder="e.g. 5" />
        </div>
        <div class="input-group">
          <label for="indian">% Indian/Asian Employees</label>
          <input type="number" id="indian" placeholder="e.g. 5" />
        </div>
      </div>

      <div class="input-row">
        <div class="input-group">
          <label for="women">% Women</label>
          <input type="number" id="women" placeholder="e.g. 40" />
        </div>
        <div class="input-group">
          <label for="men">% Men</label>
          <input type="number" id="men" placeholder="e.g. 60" />
        </div>
      </div>

      <div class="input-row">
        <div class="input-group">
          <label for="salaryBlack">Avg Annual Salary (Black)</label>
          <input type="number" id="salaryBlack" placeholder="e.g. 300000" />
        </div>
        <div class="input-group">
          <label for="salaryWhite">Avg Annual Salary (White)</label>
          <input type="number" id="salaryWhite" placeholder="e.g. 500000" />
        </div>
        <div class="input-group">
          <label for="salaryColoured">Avg Annual Salary (Coloured)</label>
          <input type="number" id="salaryColoured" placeholder="e.g. 350000" />
        </div>
        <div class="input-group">
          <label for="salaryIndian">Avg Annual Salary (Indian/Asian)</label>
          <input type="number" id="salaryIndian" placeholder="e.g. 450000" />
        </div>
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

      <div class="button-group">
        <button onclick="calculateCostSaving()">Calculate</button>
        <button class="reset-btn" onclick="window.location.reload()">Reset Calculator</button>
        <button class="download-btn" onclick="window.print()">Download as PDF</button>
        <button class="enquiry-btn" onclick="document.getElementById('enquiryForm').style.display = 'block'">Enquire About Our Solution</button>
      </div>

      <div id="enquiryForm" style="display: none; margin-top: 2rem;">
        <form action="https://formspree.io/f/manjzgjr" method="POST" style="display: flex; flex-direction: column; gap: 0.75rem;">
          <input type="text" name="name" placeholder="Your Name" required />
          <input type="text" name="organisation" placeholder="Organisation" required />
          <input type="email" name="email" placeholder="Email Address" required />
          <textarea name="message" readonly>I would like to find out more about your psychological safety and workplace culture programme.</textarea>
          <button type="submit" style="background-color: #ffb002; color: black; border: none; padding: 1rem; border-radius: 30px; font-family: 'Montserrat', sans-serif; font-size: 1rem; cursor: pointer;">Send Enquiry</button>
        </form>
      </div>

    </div>

    <div class="results-box">
      <h2>Estimated Cost Saving</h2>
      <div id="results"></div>
    </div>
  </div>
</body>
</html>
