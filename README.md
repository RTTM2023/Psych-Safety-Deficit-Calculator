<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Psychological Safety Cost Calculator</title>
  <style>
    /* --- HIDE GITHUB THEME ELEMENTS --- */
    section.page-header, 
    header.page-header, 
    .site-header,
    .project-name, 
    .project-tagline { 
      display: none !important; 
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
      gap: 1.5rem;
      align-items: flex-start;
      padding: 2rem;
      max-width: 1200px;
      margin: 0 auto;
    }

    /* Desktop Widths */
    .container {
      width: 100%;
      max-width: 550px;
      flex-shrink: 1;
      transition: max-width 0.3s ease;
    }

    .container.shrink { 
      max-width: 440px; 
    }

    .result-wrapper {
      flex: 1;
      min-width: 340px;
      background-color: #5700ff;
      color: white;
      min-height: 300px;
      align-self: flex-start;
      display: none;
      border-radius: 20px;
      padding: 2rem;
      box-sizing: border-box;
    }

    .card, .subcard {
      border-radius: 24px;
      padding: 2rem;
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
      border-bottom: 2px solid white;
      padding-bottom: 0.5rem;
    }

    .result-line { display: flex; justify-content: space-between; margin: 0.4rem 0; font-size: 0.95rem; }
    .result-line.total-divider {
      margin-top: 1.5rem;
      border-top: 2px dotted white;
      border-bottom: 2px dotted white;
      padding: 1rem 0;
      font-weight: bold;
    }

    .result-buttons { margin-top: 2rem; display: flex; flex-direction: column; gap: 1rem; }
    .result-buttons .primary { background-color: white; color: #5700ff; border: 2px dotted #ea0b82; border-radius: 999px; }
    .result-buttons .secondary { background-color: #ea0b82; color: white; border-radius: 999px; }

    /* Tooltip Fixes */
    .tooltip { position: relative; display: inline-block; vertical-align: middle; margin-left: 4px; }
    .tooltip img { 
      width: 18px; 
      height: 18px; 
      background: transparent !important; /* Forces transparency */
      display: block;
      border: none;
    }
    .tooltip:hover::after {
      content: attr(data-tooltip);
      position: absolute;
      background: rgba(0,0,0,0.85);
      color: #fff; padding: 0.6rem 0.8rem; border-radius: 5px; top: 120%; left: 50%;
      transform: translateX(-50%); display: block; max-width: 240px; width: max-content; min-width: 120px;
      white-space: normal; font-size: 0.8rem; z-index: 999;
    }

    select {
      appearance: none;
      background-image: url('data:image/svg+xml;utf8,<svg fill="black" height="24" viewBox="0 0 24 24" width="24" xmlns="http://www.w3.org/2000/svg"><path d="M7 10l5 5 5-5z"/></svg>');
      background-repeat: no-repeat; background-position: right 1rem center; background-size: 1rem;
    }

    #error-message { color: #a80000; background-color: #fdecea; padding: 1rem; border-radius: 10px; display: none; margin-top: 1rem; }
    .input-error { border: 2px solid #ea0b82 !important; }

    @media (max-width: 768px) {
      .main-wrapper { flex-direction: column; padding: 1rem; }
      .container, .container.shrink { max-width: 100%; }
      .result-wrapper { width: 100%; min-width: 100%; }
      .grid { grid-template-columns: 1fr; }
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
          <span class="tooltip" data-tooltip="Self-assessment based on feedback or surveys.">
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

      <button onclick="calculateCosts()">Calculate</button>
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

  <div id="enquiryModal" style="display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.5); z-index:1000; justify-content:center; align-items:center;">
    <div style="background:white; padding:2rem; border-radius:20px; max-width:500px; width:90%; position:relative;">
      <button onclick="closeModal()" style="position:absolute; top:10px; right:15px; background:none; color:#5700ff; font-size:24px; width:auto; padding:0;">&times;</button>
      <h2>Enquire About Our Solution</h2>
      <form action="https://formspree.io/f/movlkdbj" method="POST">
        <input type="text" name="name" placeholder="Name" required style="border:1px solid #ccc; margin-bottom:1rem;">
        <input type="email" name="email" placeholder="Email" required style="border:1px solid #ccc; margin-bottom:1rem;">
        <textarea name="message" placeholder="Message" rows="4" style="width:100%; border:1px solid #ccc; border-radius:20px; padding:1rem; margin-bottom:1rem;"></textarea>
        <button type="submit">Send</button>
      </form>
    </div>
  </div>

  <div id="emailModal" style="display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.5); z-index:1000; justify-content:center; align-items:center;">
    <div style="background:white; padding:2rem; border-radius:20px; max-width:500px; width:90%; position:relative;">
      <button onclick="closeEmailModal()" style="position:absolute; top:10px; right:15px; background:none; color:#5700ff; font-size:24px; width:auto; padding:0;">&times;</button>
      <h2>Get Your Report</h2>
      <form action="https://formspree.io/f/mzzvyqoa" method="POST">
        <input type="text" name="firstName" placeholder="First Name" required style="border:1px solid #ccc; margin-bottom:1rem;">
        <input type="email" name="email" placeholder="Email" required style="border:1px solid #ccc; margin-bottom:1rem;">
        <input type="hidden" name="totalCost" id="hiddenTotalCost">
        <button type="submit">Send Report</button>
      </form>
    </div>
  </div>

  <script>
    function getRates(level) {
      const rates = {
        low: { turnoverRates: { black: {m:0.07, w:0.08}, white: {m:0.01, w:0.015}, coloured: {m:0.03, w:0.04}, indianasian: {m:0.03, w:0.04} }, absenteeism: {black:2, white:0.5, coloured:1, indianasian:1}, presenteeism: {black:0.15, white:0.0375, coloured:0.09375, indianasian:0.09375} },
        medium: { turnoverRates: { black: {m:0.03, w:0.04}, white: {m:0.005, w:0.01}, coloured: {m:0.01, w:0.02}, indianasian: {m:0.01, w:0.02} }, absenteeism: {black:1, white:0.25, coloured:0.5, indianasian:0.5}, presenteeism: {black:0.075, white:0.015, coloured:0.045, indianasian:0.045} },
        high: { turnoverRates: { black: {m:0.01, w:0.02}, white: {m:0.0025, w:0.005}, coloured: {m:0.005, w:0.01}, indianasian: {m:0.005, w:0.01} }, absenteeism: {black:0.5, white:0.1, coloured:0.25, indianasian:0.25}, presenteeism: {black:0.0375, white:0.0075, coloured:0.01875, indianasian:0.01875} }
      };
      return rates[level];
    }

    function calculateCosts() {
      const culture = document.getElementById('cultureRating').value;
      const totalStaff = parseInt(document.getElementById('totalStaff').value);
      const avgSalary = parseInt(document.getElementById('avgSalary').value);

      if(!culture || isNaN(totalStaff) || isNaN(avgSalary)) {
        document.getElementById('error-message').style.display = 'block';
        document.getElementById('error-message').textContent = "Please fill in all fields correctly.";
        return;
      }

      const rates = getRates(culture);
      const getP = id => parseFloat(document.getElementById(id).value || 0) / 100;
      const rP = { black: getP('blackPct'), white: getP('whitePct'), coloured: getP('colouredPct'), indian: getP('indianasianPct') };
      const mP = getP('menPct'), wP = getP('womenPct');

      let tCost = 0, aCost = 0, pCost = 0, exits = 0;

      for (const r in rP) {
        const count = totalStaff * rP[r];
        const e = (count * mP * rates.turnoverRates[r].m) + (count * wP * rates.turnoverRates[r].w);
        exits += e;
        tCost += e * (0.5 * avgSalary);
        aCost += rates.absenteeism[r] * (avgSalary / 220) * count * 0.88;
        pCost += count * avgSalary * rates.presenteeism[r];
      }

      document.getElementById('resignations').textContent = Math.floor(exits).toLocaleString();
      document.getElementById('turnover').textContent = 'R ' + Math.round(tCost).toLocaleString();
      document.getElementById('absenteeism').textContent = 'R ' + Math.round(aCost).toLocaleString();
      document.getElementById('presenteeism').textContent = 'R ' + Math.round(pCost).toLocaleString();
      document.getElementById('total').textContent = 'R ' + Math.round(tCost + aCost + pCost).toLocaleString();

      document.getElementById('calcBox').classList.add('shrink');
      document.getElementById('resultBox').style.display = 'block';
      document.getElementById('resultBox').scrollIntoView({ behavior: 'smooth' });
    }

    function closeModal() { document.getElementById('enquiryModal').style.display = 'none'; }
    function closeEmailModal() { document.getElementById('emailModal').style.display = 'none'; }
    function openEmailModal() { document.getElementById('emailModal').style.display = 'flex'; document.getElementById('hiddenTotalCost').value = document.getElementById('total').textContent; }
    function resetCalculator() { location.reload(); }

    document.addEventListener('DOMContentLoaded', () => {
      document.getElementById('enquireBtn').onclick = () => document.getElementById('enquiryModal').style.display = 'flex';
    });

    // --- SCRIPT TO FORCE-REMOVE GITHUB TITLES ---
    (function() {
      const removeHeader = () => {
        const unwanted = document.querySelectorAll('header, .page-header, .site-header');
        unwanted.forEach(el => {
          if (!el.contains(document.getElementById('appTitle'))) {
            el.style.display = 'none';
          }
        });
      };
      removeHeader();
      window.onload = removeHeader;
    })();
  </script>
</body>
</html>
