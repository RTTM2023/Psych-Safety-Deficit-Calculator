<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0" />
  <title>Psychological Safety Cost Calculator</title>
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;700&display=swap" rel="stylesheet" />
  <style>
    /* 1. NUCLEAR RESET & GITHUB HEADER CLEANUP */
    * { box-sizing: border-box; }
    body {
      font-family: 'Montserrat', sans-serif;
      margin: 0;
      background-color: transparent;
      overflow-x: hidden;
      width: 100%;
    }

    #appTitle { display: block !important; }
    section.page-header, header.page-header, .page-header, header[role="banner"] {
      display: none !important;
    }

    /* 2. MAIN WRAPPER & CONTAINER RESPONSIVENESS */
    .main-wrapper {
      display: flex;
      flex-direction: column; /* Stacked for mobile */
      gap: 1rem;
      align-items: center;
      padding: 1rem;
      max-width: 1200px;
      margin: 0 auto;
    }

    @media (min-width: 1024px) {
      .main-wrapper {
        flex-direction: row;
        align-items: flex-start;
        padding: 2rem;
      }
      .container {
        width: 580px;
        flex-shrink: 0;
        transition: width 0.3s ease;
      }
      .container.shrink { width: 480px; }
      .result-wrapper { flex: 1; }
    }

    .container { width: 100%; }

    .result-wrapper {
      width: 100%;
      background-color: #5700ff;
      color: white;
      min-height: 300px;
      display: none;
      border-radius: 20px;
      padding: 2rem;
      box-sizing: border-box;
    }

    /* 3. CARD STYLE RESTORATION */
    .card, .subcard {
      border-radius: 24px;
      padding: 1.5rem; /* Mobile padding */
      margin-bottom: 1rem;
      border: 1px solid #E3C8F7;
      width: 100%;
    }
    
    @media (min-width: 1024px) {
        .card, .subcard { padding: 2rem; margin-bottom: 1rem; }
    }

    .card { background-color: white; }
    .purple-card { background-color: #e4c8f7; }

    h1 { font-size: 1.75rem; font-weight: 700; margin-bottom: 0.5rem; }
    h2 { font-size: 1.1rem; font-weight: 700; margin-top: 1rem; margin-bottom: 0.5rem; }

    label { font-weight: 500; font-size: 0.9rem; display: block; margin-bottom: 0.25rem; }

    input, select {
      width: 100%;
      padding: 0.75rem;
      border: none;
      border-radius: 30px;
      font-family: 'Montserrat', sans-serif;
      font-size: 16px;
    }

    .purple-card input, .purple-card select { background-color: white; }
    .card input, .card select { background-color: #E3C8F7; }

    .grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 1rem; }

    .pink-line { height: 2px; background-color: #ea0b82; width: 50%; margin: 1.5rem 0; }

    button.calculate {
      width: 100%;
      background-color: #5200ff;
      color: white;
      font-weight: 700;
      padding: 1rem;
      border: none;
      border-radius: 20px;
      font-size: 1rem;
      cursor: pointer;
      font-family: 'Montserrat', sans-serif;
    }

    /* 4. RESULT STYLING RESTORATION */
    .result-wrapper h2 {
      font-size: 1.2rem;
      font-weight: 700;
      margin-bottom: 1rem;
      border-bottom: 2px solid white;
      padding-bottom: 0.5rem;
    }

    .result-line { 
      display: flex; 
      justify-content: space-between; 
      margin: 0.4rem 0; 
      font-size: 0.95rem; 
      gap: 10px;
    }

    .result-line.total-divider {
      margin: 1.5rem 0 0.5rem 0;
      border-top: 2px dotted white;
      border-bottom: 2px dotted white;
      padding: 1rem 0;
      font-size: 1.05rem;
      font-weight: bold;
    }

    .result-buttons { margin-top: 2rem; display: flex; flex-direction: column; gap: 1rem; }
    
    .result-buttons .primary {
      background-color: white; color: #5700ff; border: 2px dotted #ea0b82;
      font-weight: 500; padding: 1rem 1.5rem; border-radius: 999px; font-size: 1rem; 
      cursor: pointer; font-family: 'Montserrat', sans-serif;
    }

    .result-buttons .secondary {
      background-color: #ea0b82; color: white; border: none; font-weight: 500;
      padding: 1rem 1.5rem; border-radius: 999px; font-size: 1rem; 
      cursor: pointer; font-family: 'Montserrat', sans-serif;
    }

    /* 5. TOOLTIPS & MODALS */
    .tooltip { position: relative; cursor: pointer; vertical-align: super; top: -0.1em; }
    .tooltip img { width: 14px; height: 14px; background: transparent !important; }
    .tooltip:hover::after {
      content: attr(data-tooltip);
      position: absolute;
      background: rgba(0,0,0,0.85);
      color: #fff; padding: 0.6rem 0.8rem; border-radius: 5px; top: 120%; left: 50%;
      transform: translateX(-50%); display: block; width: 200px; font-size: 0.8rem; z-index: 999;
    }

    #error-message {
      color: #a80000; background-color: #fdecea; border: 1px solid #f5c2c0;
      padding: 1rem; border-radius: 10px; margin-top: 1rem; display: none; font-size: 0.9rem;
    }

    .modal-content { 
        position: relative; background: white; padding: 2rem; border-radius: 20px; 
        max-width: 500px; width: 90%; font-family: 'Montserrat', sans-serif; 
    }
    .modal-close { 
        position: absolute; top: 10px; right: 12px; border: none; background: none;
        font-size: 28px; color: #5700ff; cursor: pointer; 
    }
  </style>
</head>
<body>
  <div class="main-wrapper">
    <div class="container" id="calcBox">
      <div class="card">
        <h1 id="appTitle">How Much Does Psychological (Un)Safety Cost Us?</h1>
      </div>

      <div class="card purple-card">
        <h2>Total Staff Complement</h2>
        <input type="text" id="totalStaff" placeholder="e.g. 1000" />
        <div class="pink-line"></div>

        <div class="grid">
          <div><label>% Black</label><input type="number" id="blackPct" /></div>
          <div><label>% White</label><input type="number" id="whitePct" /></div>
          <div><label>% Coloured</label><input type="number" id="colouredPct" /></div>
          <div><label>% Indian/Asian</label><input type="number" id="indianasianPct" /></div>
        </div>

        <div class="pink-line"></div>
        <div class="grid">
          <div><label>% Women</label><input type="number" id="womenPct" /></div>
          <div><label>% Men</label><input type="number" id="menPct" /></div>
        </div>
      </div>

      <div class="card">
        <h2>Average Annual Salary</h2>
        <input type="text" id="avgSalary" placeholder="e.g. 350000" />
      </div>

      <div class="card purple-card">
        <h2>
          Current Psych-Safety Level
          <span class="tooltip" data-tooltip="Self-assessment based on employee feedback, surveys, or observed behaviours.">
            <img src="Untitled design.svg" alt="info icon" />
          </span>
        </h2>
        <select id="cultureRating">
          <option value="" disabled selected hidden>Select</option>
          <option value="low">Low</option>
          <option value="medium">Medium</option>
          <option value="high">High</option>
        </select>
      </div>

      <button class="calculate" onclick="calculateCosts()">Calculate</button>
      <div id="error-message"></div>
    </div>

    <div class="result-wrapper" id="resultBox">
      <h2>Estimated Annual Financial Losses</h2>

      <div class="result-line">
        <span>Excess Resignations:</span>
        <span id="resignations">XXX</span>
      </div>

      <div class="result-line">
        <span>Cost of Turnover:</span>
        <span id="turnover">RXXX</span>
      </div>

      <div class="result-line">
        <span>Absenteeism Cost:</span>
        <span id="absenteeism">RXXX</span>
      </div>

      <div class="result-line">
        <span>Underperformance:</span>
        <span id="presenteeism">RXXX</span>
      </div>

      <div class="result-line total-divider"><span>Total Estimated Cost</span><span id="total">RXXX</span></div>

      <div class="result-buttons">
        <button class="primary" onclick="openEmailModal()">Email Me This Report</button>
        <button class="primary" id="enquireBtn">Enquire about our Solution</button>
        <button class="secondary" onclick="resetCalculator()">Reset Calculator</button>
      </div>
    </div>
  </div>

  </body>
</html>
