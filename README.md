<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0" />
  <title>Psychological Safety Cost Calculator</title>
  <style>
    /* 1. NUCLEAR RESET & MOBILE OVERRIDES */
    * {
      box-sizing: border-box;
      max-width: 100%;
    }

    body {
      font-family: 'Montserrat', sans-serif;
      margin: 0;
      padding: 0;
      background-color: transparent;
      overflow-x: hidden;
      width: 100%;
    }

    /* GitHub/Jekyll Header Cleanup */
    #appTitle { display: block !important; }
    section.page-header, header.page-header, .page-header, header[role="banner"] {
      display: none !important;
    }

    .main-wrapper {
      display: flex;
      flex-direction: column; /* Default to vertical for mobile */
      gap: 1.5rem;
      padding: 1rem;
      max-width: 1200px;
      margin: 0 auto;
    }

    /* 2. DESKTOP LAYOUT (1024px+) */
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
      .result-wrapper {
        flex: 1;
        margin-top: 0;
      }
    }

    /* Box width for mobile */
    .container {
      width: 100%;
    }

    .result-wrapper {
      width: 100%;
      background-color: #5200ff;
      color: white;
      min-height: 300px;
      display: none;
      border-radius: 20px;
      padding: 1.5rem;
      box-sizing: border-box;
      margin-top: 1rem;
    }

    .card, .subcard {
      border-radius: 24px;
      padding: 1.5rem;
      margin-bottom: 1rem;
      border: 1px solid #E3C8F7;
      width: 100%;
    }
    
    @media (min-width: 1024px) {
        .card, .subcard { padding: 2rem; margin-bottom: 0; }
    }

    .card { background-color: white; }
    .purple-card { background-color: #e4c8f7; }

    h1 { font-size: 1.5rem; font-weight: 700; margin-bottom: 0.5rem; line-height: 1.2; }
    h2 { font-size: 1rem; font-weight: 700; margin-top: 1rem; margin-bottom: 0.5rem; }

    label { font-weight: 500; font-size: 0.85rem; display: block; margin-bottom: 0.25rem; }

    input, select {
      width: 100%;
      padding: 0.75rem;
      border: none;
      border-radius: 30px;
      font-family: 'Montserrat', sans-serif;
      font-size: 16px; /* Best for mobile inputs */
    }

    .purple-card input, .purple-card select { background-color: white; }
    .card input, .card select { background-color: #E3C8F7; }

    /* Responsive Grid for mobile */
    .grid { 
      display: grid; 
      grid-template-columns: repeat(2, 1fr); 
      gap: 0.75rem; 
    }

    .pink-line { height: 2px; background-color: #ea0b82; width: 50%; margin: 1.2rem 0; }

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

    /* Result styling */
    .result-line { 
      display: flex; 
      flex-direction: column; /* Stack label and cost on tiny mobile */
      gap: 5px;
      margin: 1rem 0; 
      font-size: 0.9rem; 
    }

    @media (min-width: 480px) {
        .result-line { flex-direction: row; justify-content: space-between; align-items: flex-start; }
    }

    .result-line.total-divider {
      margin: 1.5rem 0;
      border-top: 2px dotted white;
      border-bottom: 2px dotted white;
      padding: 1rem 0;
      font-size: 1.1rem;
      font-weight: bold;
      flex-direction: row;
      justify-content: space-between;
    }

    /* Tooltips Mobile Fix */
    .tooltip { position: relative; cursor: pointer; }
    .tooltip img { width: 14px; vertical-align: middle; }
    .tooltip:hover::after {
      content: attr(data-tooltip);
      position: absolute;
      background: rgba(0,0,0,0.9);
      color: #fff; padding: 0.6rem; border-radius: 5px; top: 25px; left: 50%;
      transform: translateX(-50%); width: 200px; font-size: 0.75rem; z-index: 999;
    }

    .result-buttons { margin-top: 1.5rem; display: flex; flex-direction: column; gap: 0.75rem; }
    .result-buttons button { font-size: 0.9rem; padding: 0.8rem; border-radius: 999px; }

    /* Modal Mobile Cleanup */
    .modal-content {
      background: white; 
      padding: 1.5rem; 
      border-radius: 20px; 
      width: 95%; 
      max-width: 500px;
      margin: auto;
    }

    #error-message {
      color: #a80000; background-color: #fdecea; border: 1px solid #f5c2c0;
      padding: 1rem; border-radius: 10px; margin-top: 1rem; display: none; font-size: 0.85rem;
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
        <h2>Psychological Safety Level <span class="tooltip" data-tooltip="Self-assessment based on feedback, surveys, or observed behaviours."><img src="Untitled design.svg" /></span></h2>
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
      <h2>Estimated Annual Losses</h2>
      <div class="result-line"><span>Excess Resignations:</span><span id="resignations">XXX</span></div>
      <div class="result-line"><span>Turnover Cost:</span><span id="turnover">RXXX</span></div>
      <div class="result-line"><span>Absenteeism Cost:</span><span id="absenteeism">RXXX</span></div>
      <div class="result-line"><span>Underperformance Cost:</span><span id="presenteeism">RXXX</span></div>
      <div class="result-line total-divider"><span>Total Cost</span><span id="total">RXXX</span></div>

      <div class="result-buttons">
        <button class="primary" onclick="openEmailModal()">Email Me This Report</button>
        <button class="primary" id="enquireBtn">Enquire about Solution</button>
        <button class="secondary" onclick="resetCalculator()">Reset</button>
      </div>
    </div>
  </div>

  <div id="enquiryModal" style="display:none; position:fixed; top:0; left:0; width:100%; height:100%; background-color:rgba(0,0,0,0.5); z-index:1000; justify-content:center; align-items:center;">
    <div class="modal-content">
      <button class="modal-close" style="float:right; cursor:pointer;" onclick="closeModal()">&times;</button>
      <h2>Enquire Now</h2>
      <form id="enquiryForm" action="https://formspree.io/f/movlkdbj" method="POST">
        <input type="text" name="name" placeholder="Name" required style="margin-bottom:10px; border:1px solid #ccc;"/>
        <input type="email" name="email" placeholder="Email" required style="margin-bottom:10px; border:1px solid #ccc;"/>
        <textarea name="message" rows="4" placeholder="Message" required style="width:100%; border-radius:10px; border:1px solid #ccc;"></textarea>
        <button type="submit" style="margin-top:10px;">Send</button>
      </form>
    </div>
  </div>

  <div id="emailModal" style="display:none; position:fixed; top:0; left:0; width:100%; height:100%; background-color:rgba(0,0,0,0.5); z-index:1000; justify-content:center; align-items:center;">
    <div class="modal-content">
      <button class="modal-close" style="float:right; cursor:pointer;" onclick="closeEmailModal()">&times;</button>
      <h2>Email My Report</h2>
      <form id="emailForm" action="https://formspree.io/f/mzzvyqoa" method="POST">
        <input type="text" name="firstName" placeholder="First Name" required style="margin-bottom:10px; border:1px solid #ccc;"/>
        <input type="email" name="email" placeholder="Email" required style="margin-bottom:10px; border:1px solid #ccc;"/>
        <input type="hidden" name="totalCost" id="hiddenTotalCost" />
        <button type="submit" style="margin-top:10px;">Send Report</button>
      </form>
    </div>
  </div>

<script>
  // COPIED YOUR EXACT LOGIC BELOW
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
    const totalStaffRaw = document.getElementById('totalStaff').value;
    const salaryRaw = document.getElementById('avgSalary').value;
    const culture = document.getElementById('cultureRating').value;
    const errorBox = document.getElementById('error-message');

    if (!totalStaffRaw || !salaryRaw || !culture) {
      errorBox.textContent = "Please fill in all fields.";
      errorBox.style.display = "block";
      return;
    }

    const total = parseInt(totalStaffRaw);
    const avgSalary = parseInt(salaryRaw);
    const { turnoverRates, absenteeismDays, presenteeismRates } = getRates(culture);

    const getPct = id => parseFloat(document.getElementById(id).value || 0) / 100;
    const raceGroups = { black: getPct('blackPct'), white: getPct('whitePct'), coloured: getPct('colouredPct'), indianasian: getPct('indianasianPct') };
    const genderSplit = { men: getPct('menPct'), women: getPct('womenPct') };

    let turnoverCost = 0, absenteeismCost = 0, presenteeismCost = 0, totalExits = 0;

    for (const [race, pct] of Object.entries(raceGroups)) {
      const headcount = total * pct;
      const maleHeadcount = headcount * genderSplit.men;
      const femaleHeadcount = headcount * genderSplit.women;
      const exits = (maleHeadcount * turnoverRates[race].men) + (femaleHeadcount * turnoverRates[race].women);
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

    if (window.innerWidth >= 1024) document.getElementById('calcBox').classList.add('shrink');
    document.getElementById('resultBox').style.display = 'block';
    document.getElementById('resultBox').scrollIntoView({ behavior: 'smooth' });
  }

  function openEmailModal() { document.getElementById('emailModal').style.display = 'flex'; }
  function closeEmailModal() { document.getElementById('emailModal').style.display = 'none'; }
  function closeModal() { document.getElementById('enquiryModal').style.display = 'none'; }
  function resetCalculator() { window.location.reload(); }

  document.getElementById('enquireBtn').addEventListener('click', () => { document.getElementById('enquiryModal').style.display = 'flex'; });
</script>
</body>
</html>
