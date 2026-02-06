<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Psychological Safety Cost Calculator</title>
  <style>
    /* 1. NUCLEAR HIDE FOR GITHUB HEADERS */
    section.page-header, 
    header.page-header, 
    .site-header,
    [role="banner"],
    .project-name, 
    .project-tagline { 
      display: none !important; 
      height: 0 !important;
      padding: 0 !important;
      margin: 0 !important;
      visibility: hidden !important;
    }

    body {
      font-family: 'Montserrat', sans-serif;
      margin: 0;
      background-color: transparent;
    }

    * { box-sizing: border-box; }
    html, body { max-width: 100%; overflow-x: hidden; } 

    #appTitle { display: block !important; }

    .main-wrapper {
      display: flex;
      gap: 1rem;
      align-items: flex-start;
      padding: 2rem;
      max-width: 1200px;
      margin: 0 auto;
    }

    /* FIX: ADJUSTED WIDTHS FOR COMPUTER VIEW */
    .container {
      width: 100%;
      max-width: 550px; /* Changed from fixed 580px */
      flex-shrink: 1;    /* Allows it to yield space */
      transition: all 0.3s ease;
    }

    .container.shrink { 
      max-width: 440px; /* Changed from 480px to give more space to result box */
    }

    .result-wrapper {
      flex: 1;
      background-color: #5700ff;
      color: white;
      min-height: 300px;
      align-self: flex-start;
      display: none;
      border-radius: 20px;
      padding: 2rem;
      box-sizing: border-box;
      min-width: 320px; /* Ensures it doesn't get too squashed */
    }

    .card, .subcard {
      border-radius: 24px;
      padding: 2rem;
      margin-bottom: 0;
      border: 1px solid #E3C8F7;
    }
    .card { background-color: white; }
    .purple-card { background-color: #e4c8f7; }

    h1 { font-size: 1.75rem; font-weight: 700; margin-bottom: 0.5rem; }
    h2 { font-size: 0.8rem; font-weight: 700; margin-top: 1rem; margin-bottom: 0.5rem; }

    label { font-weight: 500; font-size: 0.9rem; display: block; margin-bottom: 0.25rem; }

    input, select {
      width: 100%;
      padding: 0.75rem;
      margin-bottom: 0;
      border: none;
      border-radius: 30px;
      font-family: 'Montserrat', sans-serif;
    }

    .purple-card input, .purple-card select { background-color: white; }
    .card input, .card select { background-color: #E3C8F7; }

    .grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 1rem; }
    .pink-line { height: 2px; background-color: #ea0b82; width: 50%; margin: 1.5rem 0; }

    button {
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

    .result-wrapper h2 {
      font-size: 1.2rem;
      font-weight: 700;
      margin-bottom: 1rem;
      border-bottom: 2px solid white;
      padding-bottom: 0.5rem;
    }

    .result-line { display: flex; justify-content: space-between; margin: 0.4rem 0; font-size: 0.95rem; }

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
      font-weight: 500; padding: 1rem 1.5rem; border-radius: 999px; font-size: 1rem; cursor: pointer;
    }
    .result-buttons .secondary {
      background-color: #ea0b82; color: white; border: none; font-weight: 500;
      padding: 1rem 1.5rem; border-radius: 999px; font-size: 1rem; cursor: pointer;
    }

    /* FIX: TRANSPARENT TOOLTIP ICONS */
    .tooltip { position: relative; display: inline-block; vertical-align: super; margin-left: 2px; top: -0.2em; }
    .tooltip img { 
        width: 14px; 
        height: 14px; 
        display: inline; 
        margin: 0; 
        padding: 0; 
        background: transparent !important; /* Force transparency */
        border: none !important;
        vertical-align: middle; 
    }
    
    .tooltip:hover::after {
      content: attr(data-tooltip);
      position: absolute;
      background: rgba(0,0,0,0.85);
      color: #fff; padding: 0.6rem 0.8rem; border-radius: 5px; top: 120%; left: 50%;
      transform: translateX(-50%); display: block; max-width: 240px; width: max-content; min-width: 120px;
      white-space: normal; font-size: 0.8rem; z-index: 999; text-align: left;
    }

    select {
      appearance: none; -webkit-appearance: none; -moz-appearance: none;
      padding-right: 2.5rem;
      background-image: url('data:image/svg+xml;utf8,<svg fill="black" height="24" viewBox="0 0 24 24" width="24" xmlns="http://www.w3.org/2000/svg"><path d="M7 10l5 5 5-5z"/></svg>');
      background-repeat: no-repeat; background-position: right 1rem center; background-size: 1rem;
    }

    #error-message {
      color: #a80000; background-color: #fdecea; border: 1px solid #f5c2c0;
      padding: 1rem; border-radius: 10px; margin-bottom: 1rem; display: none; font-size: 0.9rem;
    }
    .input-error { border: 2px solid #ea0b82 !important; background-color: #fff0f5 !important; }

    /* ========================
       Mobile / Small screens
       ======================== */
    @media (max-width: 768px) {
      .main-wrapper { flex-direction: column; gap: 0.75rem; padding: 1rem; max-width: 100%; }
      .container, .container.shrink { width: 100%; max-width: 100%; }
      .result-wrapper { width: 100%; margin-top: 0.75rem; border-radius: 16px; padding: 1rem; min-height: auto; }
      .grid { grid-template-columns: 1fr; gap: 0.75rem; }
      .result-line { display: grid; grid-template-columns: 1fr auto; align-items: baseline; column-gap: 0.5rem; }
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
          How would you rate your organisation's current level of psychological safety?
          <span class="tooltip" data-tooltip="This is a self-assessment of your organisation’s culture.">
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
      <h2>Estimated Financial Losses Per Year</h2>
      <div class="result-line"><span>Excess Resignations:</span><span id="resignations">XXX</span></div>
      <div class="result-line"><span>Cost of Excess Staff Turnover:</span><span id="turnover">RXXX</span></div>
      <div class="result-line"><span>Excess Sick Days/Absenteeism:</span><span id="absenteeism">RXXX</span></div>
      <div class="result-line"><span>On-the-job Underperformance:</span><span id="presenteeism">RXXX</span></div>
      <div class="result-line total-divider"><span>Total Estimated Cost</span><span id="total">RXXX</span></div>
      <div class="result-buttons">
        <button class="primary" onclick="openEmailModal()">Email Me This Report</button>
        <button class="primary" id="enquireBtn">Enquire about our Solution</button>
        <button class="secondary" onclick="resetCalculator()">Reset Calculator</button>
      </div>
    </div>
  </div>

  <script>
  // YOUR ORIGINAL CALCULATION LOGIC
  function getRates(level) {
    const rates = {
      low: { turnoverRates: { black: { men: 0.07, women: 0.08 }, white: { men: 0.01, women: 0.015 }, coloured: { men: 0.03, women: 0.04 }, indianasian: { men: 0.03, women: 0.04 } }, absenteeismDays: { black: 2, white: 0.5, coloured: 1, indianasian: 1 }, presenteeismRates: { black: 0.15, white: 0.0375, coloured: 0.09375, indianasian: 0.09375 } },
      medium: { turnoverRates: { black: { men: 0.03, women: 0.04 }, white: { men: 0.005, women: 0.01 }, coloured: { men: 0.01, women: 0.02 }, indianasian: { men: 0.01, women: 0.02 } }, absenteeismDays: { black: 1, white: 0.25, coloured: 0.5, indianasian: 0.5 }, presenteeismRates: { black: 0.075, white: 0.015, coloured: 0.045, indianasian: 0.045 } },
      high: { turnoverRates: { black: { men: 0.01, women: 0.02 }, white: { men: 0.0025, women: 0.005 }, coloured: { men: 0.005, women: 0.01 }, indianasian: { men: 0.005, women: 0.01 } }, absenteeismDays: { black: 0.5, white: 0.1, coloured: 0.25, indianasian: 0.25 }, presenteeismRates: { black: 0.0375, white: 0.0075, coloured: 0.01875, indianasian: 0.01875 } }
    };
    return rates[level];
  }

  function calculateCosts() {
    const select = document.getElementById('cultureRating');
    const culture = select.value;
    const salaryEl = document.getElementById('avgSalary');
    const totalStaffEl = document.getElementById('totalStaff');
    
    if (!culture || !salaryEl.value || !totalStaffEl.value) return;

    const total = parseInt(totalStaffEl.value);
    const avgSalary = parseInt(salaryEl.value);
    const rates = getRates(culture);

    const getPct = id => parseFloat(document.getElementById(id).value || 0) / 100;
    const racePct = { black: getPct('blackPct'), white: getPct('whitePct'), coloured: getPct('colouredPct'), indianasian: getPct('indianasianPct') };
    const genderSplit = { men: getPct('menPct'), women: getPct('womenPct') };

    let tCost = 0, aCost = 0, pCost = 0, exits = 0;

    for (const r in racePct) {
      const headcount = total * racePct[r];
      const e = (headcount * genderSplit.men * rates.turnoverRates[r].men) + (headcount * genderSplit.women * rates.turnoverRates[r].women);
      exits += e;
      tCost += e * (0.5 * avgSalary);
      aCost += rates.absenteeismDays[r] * (avgSalary / 220) * headcount * 0.88;
      pCost += headcount * avgSalary * rates.presenteeismRates[r];
    }

    document.getElementById('resignations').textContent = Math.floor(exits).toLocaleString();
    document.getElementById('turnover').textContent = 'R ' + Math.round(tCost).toLocaleString();
    document.getElementById('absenteeism').textContent = 'R ' + Math.round(aCost).toLocaleString();
    document.getElementById('presenteeism').textContent = 'R ' + Math.round(pCost).toLocaleString();
    document.getElementById('total').textContent = 'R ' + Math.round(tCost + aCost + pCost).toLocaleString();

    document.getElementById('calcBox').classList.add('shrink');
    document.getElementById('resultBox').style.display = 'block';
  }

  function resetCalculator() { location.reload(); }

  // 2. JAVASCRIPT FIX TO REMOVE GITHUB THEME TITLE
  window.addEventListener('load', function() {
    const removeHeader = () => {
       const titles = document.querySelectorAll('h1, h2, section, header');
       titles.forEach(el => {
         if (el.textContent.includes('Psych-Safety-Deficit') && el.id !== 'appTitle') {
           el.style.display = 'none';
           el.remove(); 
         }
       });
    };
    removeHeader();
    setTimeout(removeHeader, 500); // Run again shortly after to catch late loaders
  });
</script>

</body>
</html>
