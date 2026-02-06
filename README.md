<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Psychological Safety Cost Calculator</title>
  <style>
    body {
      font-family: 'Montserrat', sans-serif;
      margin: 0;
      background-color: transparent;
    }

    /* 1. NUCLEAR RESET & GITHUB CLEANUP */
    * { box-sizing: border-box; }
    html, body { max-width: 100%; overflow-x: hidden; }

    /* Hide the GitHub Pages/Jekyll theme header */
    section.page-header, header.page-header, .page-header, header[role="banner"], h1:first-of-type {
      display: none !important;
    }
    #appTitle { display: block !important; }

    /* 2. DESKTOP LAYOUT FIX (1024px+) */
    .main-wrapper {
      display: flex;
      flex-direction: column; /* Mobile first */
      gap: 2rem;
      align-items: center;
      padding: 1rem;
      max-width: 1400px; /* Increased to give results more room */
      margin: 0 auto;
    }

    @media (min-width: 1024px) {
      .main-wrapper {
        flex-direction: row;
        align-items: flex-start;
        padding: 2rem;
      }
      .container {
        width: 500px; /* Locked width for input box */
        flex-shrink: 0;
        transition: width 0.3s ease;
      }
      .container.shrink { width: 450px; }
      .result-wrapper {
        flex: 1; /* Result box now takes all remaining desktop space */
        display: none;
      }
    }

    /* 3. CARD & STYLE RESTORATION */
    .container { width: 100%; }

    .result-wrapper {
      width: 100%;
      background-color: #5700ff;
      color: white;
      min-height: 300px;
      border-radius: 20px;
      padding: 2.5rem;
      box-sizing: border-box;
    }

    .card, .subcard {
      border-radius: 24px;
      padding: 2rem;
      margin-bottom: 1rem;
      border: 1px solid #E3C8F7;
    }
    .card { background-color: white; }
    .purple-card { background-color: #e4c8f7; }

    h1 { font-size: 1.75rem; font-weight: 700; margin-bottom: 0.5rem; }
    h2 { font-size: 1rem; font-weight: 700; margin-top: 1rem; margin-bottom: 0.5rem; }

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
    }

    /* 4. RESULT DATA STYLING */
    .result-line { display: flex; justify-content: space-between; margin: 0.6rem 0; font-size: 0.95rem; gap: 10px; }
    
    .result-line.total-divider {
      margin: 1.5rem 0 0.5rem 0;
      border-top: 2px dotted white;
      border-bottom: 2px dotted white;
      padding: 1rem 0;
      font-size: 1.05rem;
      font-weight: bold;
    }

    /* 5. MOBILE OVERRIDES (Max 768px) */
    @media (max-width: 768px) {
      .grid { grid-template-columns: 1fr; }
      .card { padding: 1.5rem; }
      .result-line { flex-direction: column; align-items: flex-start; }
      .result-line span:last-child { font-weight: bold; font-size: 1.1rem; }
    }

    /* Tooltips */
    .tooltip { position: relative; display: inline-block; vertical-align: super; }
    .tooltip img { width: 14px; height: 14px; background: transparent !important; }

    .result-buttons { margin-top: 2rem; display: flex; flex-direction: column; gap: 1rem; }
    .result-buttons button { border-radius: 999px; padding: 1rem; cursor: pointer; font-family: 'Montserrat'; }
    .result-buttons .primary { background: white; color: #5700ff; border: 2px dotted #ea0b82; }
    .result-buttons .secondary { background: #ea0b82; color: white; border: none; }

    #error-message { color: #a80000; background: #fdecea; padding: 1rem; border-radius: 10px; display: none; margin-top: 1rem; }
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
        <input type="number" id="totalStaff" placeholder="e.g. 1000" />
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
        <h2>Average Annual Salary (ZAR)</h2>
        <input type="number" id="avgSalary" placeholder="e.g. 350000" />
      </div>

      <div class="card purple-card">
        <h2>Organisation's psych-safety rating?</h2>
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
      <h2 style="border-bottom: 2px solid white; padding-bottom: 10px;">Estimated Financial Losses</h2>
      <div class="result-line"><span>Excess Resignations:</span><span id="resignations">XXX</span></div>
      <div class="result-line"><span>Cost of Turnover:</span><span id="turnover">RXXX</span></div>
      <div class="result-line"><span>Absenteeism Cost:</span><span id="absenteeism">RXXX</span></div>
      <div class="result-line"><span>Underperformance:</span><span id="presenteeism">RXXX</span></div>
      <div class="result-line total-divider"><span>Total Estimated Cost</span><span id="total">RXXX</span></div>

      <div class="result-buttons">
        <button class="primary" onclick="openEmailModal()">Email Me This Report</button>
        <button class="secondary" onclick="window.location.reload()">Reset Calculator</button>
      </div>
    </div>
  </div>

  <script>
    function getRates(level) {
      const rates = {
        low: {
          turnoverRates: { black: { men: 0.07, women: 0.08 }, white: { men: 0.01, women: 0.015 }, coloured: { men: 0.03, women: 0.04 }, indianasian: { men: 0.03, women: 0.04 } },
          absenteeismDays: { black: 2, white: 0.5, coloured: 1, indianasian: 1 },
          presenteeismRates: { black: 0.15, white: 0.0375, coloured: 0.09375, indianasian: 0.09375 }
        },
        medium: {
          turnoverRates: { black: { men: 0.03, women: 0.04 }, white: { men: 0.005, women: 0.01 }, coloured: { men: 0.01, women: 0.02 }, indianasian: { men: 0.01, women: 0.02 } },
          absenteeismDays: { black: 1, white: 0.25, coloured: 0.5, indianasian: 0.5 },
          presenteeismRates: { black: 0.075, white: 0.015, coloured: 0.045, indianasian: 0.045 }
        },
        high: {
          turnoverRates: { black: { men: 0.01, women: 0.02 }, white: { men: 0.0025, women: 0.005 }, coloured: { men: 0.005, women: 0.01 }, indianasian: { men: 0.005, women: 0.01 } },
          absenteeismDays: { black: 0.5, white: 0.1, coloured: 0.25, indianasian: 0.25 },
          presenteeismRates: { black: 0.0375, white: 0.0075, coloured: 0.01875, indianasian: 0.01875 }
        }
      };
      return rates[level];
    }

    function calculateCosts() {
      const totalStaff = parseInt(document.getElementById('totalStaff').value);
      const avgSalary = parseInt(document.getElementById('avgSalary').value);
      const culture = document.getElementById('cultureRating').value;

      if (!totalStaff || !avgSalary || !culture) {
        document.getElementById('error-message').style.display = 'block';
        document.getElementById('error-message').textContent = 'Please fill in all main fields.';
        return;
      }

      const { turnoverRates, absenteeismDays, presenteeismRates } = getRates(culture);
      const getPct = id => (parseFloat(document.getElementById(id).value) || 0) / 100;
      
      // Basic logic check for race totals
      const raceTotal = getPct('blackPct') + getPct('whitePct') + getPct('colouredPct') + getPct('indianasianPct');
      if (raceTotal < 0.99) { alert('Race percentages should add to 100%'); return; }

      let turnoverCost = 0, absenteeismCost = 0, presenteeismCost = 0, totalExits = 0;
      const races = ['black', 'white', 'coloured', 'indianasian'];
      const menPct = getPct('menPct');
      const womenPct = getPct('womenPct');

      races.forEach(race => {
        const racePct = getPct(race + 'Pct');
        const headcount = totalStaff * racePct;
        const exits = (headcount * menPct * turnoverRates[race].men) + (headcount * womenPct * turnoverRates[race].women);
        
        totalExits += exits;
        turnoverCost += exits * (0.5 * avgSalary);
        absenteeismCost += absenteeismDays[race] * (avgSalary / 220) * headcount * 0.88;
        presenteeismCost += headcount * avgSalary * presenteeismRates[race];
      });

      const totalCost = turnoverCost + absenteeismCost + presenteeismCost;

      document.getElementById('resignations').textContent = Math.floor(totalExits).toLocaleString();
      document.getElementById('turnover').textContent = 'R ' + Math.round(turnoverCost).toLocaleString();
      document.getElementById('absenteeism').textContent = 'R ' + Math.round(absenteeismCost).toLocaleString();
      document.getElementById('presenteeism').textContent = 'R ' + Math.round(presenteeismCost).toLocaleString();
      document.getElementById('total').textContent = 'R ' + Math.round(totalCost).toLocaleString();

      if (window.innerWidth >= 1024) document.getElementById('calcBox').classList.add('shrink');
      document.getElementById('resultBox').style.display = 'block';
      document.getElementById('resultBox').scrollIntoView({ behavior: 'smooth' });
    }
  </script>
</body>
</html>
