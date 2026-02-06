<!DOCTYPE html>
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

    /* Mobile base */
    * { box-sizing: border-box; }
    html, body { max-width: 100%; overflow-x: hidden; }

    /* Keep app title visible */
    #appTitle { display: block !important; }

    /* Hide GitHub Pages theme headers */
    section.page-header,
    header.page-header,
    .page-header .project-name,
    .page-header .project-tagline,
    header[role="banner"] {
      display: none !important;
    }

    .main-wrapper {
      display: flex;
      gap: 1.5rem;
      align-items: flex-start;
      padding: 2rem;
      max-width: 1200px;
      margin: 0 auto;
    }

    /* --- UPDATED DESKTOP WIDTHS --- */
    .container {
      width: 100%;
      max-width: 550px;      /* Slightly narrower base for better desktop fit */
      flex-shrink: 1;        /* Allows the calc box to yield space if needed */
      transition: max-width 0.3s ease;
    }

    /* When the result box appears, we shrink the calculator to allow side-by-side view */
    .container.shrink { 
      max-width: 440px; 
    }

    .result-wrapper {
      flex: 1;
      min-width: 340px;      /* Ensures the results remain readable on smaller laptops */
      background-color: #5700ff;
      color: white;
      min-height: 300px;
      align-self: flex-start;
      display: none;
      border-radius: 20px;
      padding: 2rem;
      box-sizing: border-box;
    }

    /* UI Components */
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

    /* Tooltips */
    .tooltip { position: relative; display: inline-block; vertical-align: super; margin-left: 2px; top: -0.2em; }
    .tooltip img { width: 14px; height: 14px; display: inline; vertical-align: middle; }
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

    /* Modal Styling */
    .modal-content { position: relative; }
    .modal-close {
      position: absolute; top: 10px; right: 12px; border: none; background: none;
      font-size: 28px; color: #5700ff; cursor: pointer; line-height: 1; padding: 0;
    }

    /* ========================
       Mobile / Small screens
       ======================== */
    @media (max-width: 768px) {
      .main-wrapper {
        flex-direction: column;
        gap: 0.75rem;
        padding: 1rem;
        max-width: 100%;
      }

      .container, .container.shrink { 
        width: 100%; 
        max-width: 100%; 
      }

      .result-wrapper {
        width: 100%;
        margin-top: 0.75rem;
        border-radius: 16px;
        padding: 1rem;
        min-height: auto;
      }

      .result-line {
        display: grid;
        grid-template-columns: 1fr auto;
        align-items: baseline;
        column-gap: 0.5rem;
      }

      .grid { grid-template-columns: 1fr; gap: 0.75rem; }

      input, select, button, textarea {
        font-size: 16px;
        padding: 0.9rem;
      }
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
          <span class="tooltip" data-tooltip="Self-assessment based on feedback, surveys, or observed behaviours.">
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

      <div class="result-line">
        <span>Excess Resignations:</span>
        <span id="resignations">XXX</span>
      </div>

      <div class="result-line">
        <span>Cost of Excess Staff Turnover:</span>
        <span id="turnover">RXXX</span>
      </div>

      <div class="result-line">
        <span>Excess Sick Days/Absenteeism:</span>
        <span id="absenteeism">RXXX</span>
      </div>

      <div class="result-line">
        <span>On-the-job Underperformance:</span>
        <span id="presenteeism">RXXX</span>
      </div>

      <div class="result-line total-divider">
        <span>Total Estimated Cost</span>
        <span id="total">RXXX</span>
      </div>

      <div class="result-buttons">
        <button class="primary" onclick="openEmailModal()">Email Me This Report</button>
        <button class="primary" id="enquireBtn">Enquire about our Solution</button>
        <button class="secondary" onclick="resetCalculator()">Reset Calculator</button>
      </div>
    </div>
  </div>

  <div id="enquiryModal" style="display:none; position:fixed; top:0; left:0; width:100%; height:100%; background-color:rgba(0,0,0,0.5); z-index:1000; justify-content:center; align-items:center;">
    <div class="modal-content" style="background:white; padding:2rem; border-radius:20px; max-width:500px; width:90%; position:relative; font-family: 'Montserrat', sans-serif;">
      <button class="modal-close" onclick="closeModal()" aria-label="Close">&times;</button>
      <h2>Enquire About Our Solution</h2>
      <form id="enquiryForm" action="https://formspree.io/f/movlkdbj" method="POST">
        <input type="hidden" name="_subject" value="Psych Safety Calculator - Enquiry">
        <label>Name</label>
        <input type="text" name="name" required style="border:1px solid #ccc; margin-bottom:1rem;">
        <label>Email</label>
        <input type="email" name="email" required style="border:1px solid #ccc; margin-bottom:1rem;">
        <label>Message</label>
        <textarea name="message" rows="4" required style="width:100%; padding:0.75rem; border-radius:20px; border:1px solid #ccc; margin-bottom:1rem;"></textarea>
        <button type="submit" style="background-color:#5700ff; color:white;">Send</button>
      </form>
    </div>
  </div>

  <div id="emailModal" style="display:none; position:fixed; top:0; left:0; width:100%; height:100%; background-color:rgba(0,0,0,0.5); z-index:1000; justify-content:center; align-items:center;">
    <div class="modal-content" style="background:white; padding:2rem; border-radius:20px; max-width:500px; width:90%; position:relative; font-family: 'Montserrat', sans-serif;">
      <button class="modal-close" onclick="closeEmailModal()" aria-label="Close">&times;</button>
      <h2>Get Your Report by Email</h2>
      <form id="emailForm" action="https://formspree.io/f/mzzvyqoa" method="POST">
        <input type="hidden" name="_subject" value="Psych Safety Calculator - Report">
        <label>First Name</label>
        <input type="text" name="firstName" required style="border:1px solid #ccc; margin-bottom:1rem;" />
        <label>Last Name</label>
        <input type="text" name="lastName" required style="border:1px solid #ccc; margin-bottom:1rem;" />
        <label>Email Address</label>
        <input type="email" name="email" required style="border:1px solid #ccc; margin-bottom:1rem;" />
        <input type="hidden" name="totalCost" id="hiddenTotalCost" />
        <button type="submit" style="background-color:#5700ff; color:white;">Send Report</button>
      </form>
    </div>
  </div>

  <script>
    function getRates(level) {
      const rates = {
        low: {
          turnoverRates: {
            black: { men: 0.07, women: 0.08 },
            white: { men: 0.01, women: 0.015 },
            coloured: { men: 0.03, women: 0.04 },
            indianasian: { men: 0.03, women: 0.04 }
          },
          absenteeismDays: { black: 2, white: 0.5, coloured: 1, indianasian: 1 },
          presenteeismRates: { black: 0.15, white: 0.0375, coloured: 0.09375, indianasian: 0.09375 }
        },
        medium: {
          turnoverRates: {
            black: { men: 0.03, women: 0.04 },
            white: { men: 0.005, women: 0.01 },
            coloured: { men: 0.01, women: 0.02 },
            indianasian: { men: 0.01, women: 0.02 }
          },
          absenteeismDays: { black: 1, white: 0.25, coloured: 0.5, indianasian: 0.5 },
          presenteeismRates: { black: 0.075, white: 0.015, coloured: 0.045, indianasian: 0.045 }
        },
        high: {
          turnoverRates: {
            black: { men: 0.01, women: 0.02 },
            white: { men: 0.0025, women: 0.005 },
            coloured: { men: 0.005, women: 0.01 },
            indianasian: { men: 0.005, women: 0.01 }
          },
          absenteeismDays: { black: 0.5, white: 0.1, coloured: 0.25, indianasian: 0.25 },
          presenteeismRates: { black: 0.0375, white: 0.0075, coloured: 0.01875, indianasian: 0.01875 }
        }
      };
      return rates[level];
    }

    function calculateCosts() {
      const errorBox = document.getElementById('error-message');
      document.querySelectorAll('.input-error').forEach(el => el.classList.remove('input-error'));
      errorBox.textContent = '';
      errorBox.style.display = 'none';

      const select = document.getElementById('cultureRating');
      const culture = select.value;
      const salaryEl = document.getElementById('avgSalary');
      const salaryRaw = salaryEl.value.trim();
      const totalStaffEl = document.getElementById('totalStaff');
      const totalStaffRaw = totalStaffEl.value.trim();

      let hasError = false;
      let message = '';

      if (!culture) { select.classList.add('input-error'); message += 'Select safety level.\n'; hasError = true; }
      if (!/^[0-9]+$/.test(totalStaffRaw)) { totalStaffEl.classList.add('input-error'); message += 'Staff must be numbers.\n'; hasError = true; }
      if (!/^[0-9]+$/.test(salaryRaw)) { salaryEl.classList.add('input-error'); message += 'Salary must be numbers.\n'; hasError = true; }

      const getPct = id => parseFloat(document.getElementById(id).value || 0) / 100;
      const raceTotal = getPct('blackPct') + getPct('whitePct') + getPct('colouredPct') + getPct('indianasianPct');
      const genderTotal = getPct('menPct') + getPct('womenPct');

      if (Math.abs(raceTotal - 1) > 0.01) { message += 'Race must sum to 100%.\n'; hasError = true; }
      if (Math.abs(genderTotal - 1) > 0.01) { message += 'Gender must sum to 100%.\n'; hasError = true; }

      if (hasError) { errorBox.textContent = message; errorBox.style.display = 'block'; return; }

      const total = parseInt(totalStaffRaw);
      const avgSalary = parseInt(salaryRaw);
      const { turnoverRates, absenteeismDays, presenteeismRates } = getRates(culture);

      let turnoverCost = 0, absenteeismCost = 0, presenteeismCost = 0, totalExits = 0;
      const racePctMap = { black: getPct('blackPct'), white: getPct('whitePct'), coloured: getPct('colouredPct'), indianasian: getPct('indianasianPct') };
      const mPct = getPct('menPct'), wPct = getPct('womenPct');

      for (const race in racePctMap) {
        const headcount = total * racePctMap[race];
        const exits = (headcount * mPct * turnoverRates[race].men) + (headcount * wPct * turnoverRates[race].women);
        totalExits += exits;
        turnoverCost += exits * (0.5 * avgSalary);
        absenteeismCost += absenteeismDays[race] * (avgSalary / 220) * headcount * 0.88;
        presenteeismCost += headcount * avgSalary * presenteeismRates[race];
      }

      const totalCost = turnoverCost + absenteeismCost + presenteeismCost;
      document.getElementById('resignations').textContent = Math.floor(totalExits).toLocaleString();
      document.getElementById('turnover').textContent = 'R ' + Math.round(turnoverCost).toLocaleString();
      document.getElementById('absenteeism').textContent = 'R ' + Math.round(absenteeismCost).toLocaleString();
      document.getElementById('presenteeism').textContent = 'R ' + Math.round(presenteeismCost).toLocaleString();
      document.getElementById('total').textContent = 'R ' + Math.round(totalCost).toLocaleString();

      document.getElementById('calcBox').classList.add('shrink');
      document.getElementById('resultBox').style.display = 'block';
      document.getElementById('resultBox').scrollIntoView({ behavior: 'smooth' });
    }

    function openEmailModal() { 
      document.getElementById('emailModal').style.display = 'flex'; 
      document.getElementById('hiddenTotalCost').value = document.getElementById('total').textContent;
    }
    function closeEmailModal() { document.getElementById('emailModal').style.display = 'none'; }
    function closeModal() { document.getElementById('enquiryModal').style.display = 'none'; }
    function resetCalculator() { location.reload(); }

    document.addEventListener('DOMContentLoaded', () => {
      document.getElementById('enquireBtn').onclick = () => document.getElementById('enquiryModal').style.display = 'flex';
      
      // Close on backdrop click
      window.onclick = (e) => {
        if (e.target.id === 'enquiryModal' || e.target.id === 'emailModal') {
          e.target.style.display = 'none';
        }
      };
    });
  </script>
</body>
</html>
