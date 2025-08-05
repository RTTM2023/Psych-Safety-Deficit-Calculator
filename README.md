<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <style>
body {
  font-family: 'Montserrat', sans-serif;
  margin: 0;
  background-color: transparent;
}

    .main-wrapper {
      display: flex;
      gap: 1rem;
      align-items: flex-start;
      padding: 2rem;
      max-width: 1200px;
      margin: 0 auto;
    }

    .container {
      width: 580px;
      flex-shrink: 0;
      transition: width 0.3s ease;
    }

    .container.shrink {
      width: 480px;
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
    }

    h2 {
      font-size: 0.8rem;
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
      margin: 1.5rem 0;
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
      cursor: pointer;
    }

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
    }

    .result-line.total-divider {
      margin: 1.5rem 0 0.5rem 0;
      border-top: 2px dotted white;
      border-bottom: 2px dotted white;
      padding: 1rem 0;
      font-size: 1.05rem;
      font-weight: bold;
    }

    .result-buttons {
      margin-top: 2rem;
      display: flex;
      flex-direction: column;
      gap: 1rem;
    }

.result-buttons .primary {
  background-color: white;
  color: #5700ff;
  border: 2px dotted #ea0b82;
  font-weight: 500; /* not bold */
  padding: 1rem 1.5rem;
  border-radius: 999px; /* pill shape */
  font-size: 1rem;
  cursor: pointer;
}

.result-buttons .secondary {
  background-color: #ea0b82;
  color: white;
  border: none;
  font-weight: 500; /* not bold */
  padding: 1rem 1.5rem;
  border-radius: 999px; /* pill shape */
  font-size: 1rem;
  cursor: pointer;
}
#enquiryModal.show {
  display: flex !important;
}
.tooltip {
  position: relative;
  display: inline-block;
  vertical-align: super; /* move icon up */
  margin-left: 2px; /* tighter spacing */
  top: -0.2em; /* fine-tune vertical position */
}

.tooltip img {
  width: 14px;
  height: 14px;
  display: inline;
  margin: 0;
  padding: 0;
  background-color: transparent;
  vertical-align: middle;
  line-height: 1;
}

.tooltip:hover::after {
  content: attr(data-tooltip);
  position: absolute;
  background: rgba(0, 0, 0, 0.85);
  color: #fff;
  padding: 0.6rem 0.8rem;
  border-radius: 5px;
  top: 120%;
  left: 50%;
  transform: translateX(-50%);
  display: block;
  max-width: 240px;
  width: max-content;
  min-width: 120px;
  white-space: normal;
  font-size: 0.8rem;
  z-index: 999;
  text-align: left;
}
.container h2 {
  font-size: 1.2rem;
  font-weight: 700;
  margin-bottom: 1rem;
  padding-bottom: 0.5rem;
  border-bottom: none; /* Explicitly remove underline */
}
h2 > a {
  display: none;
}
select {
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  padding-right: 2.5rem; /* extra space for arrow */
  background-image: url('data:image/svg+xml;utf8,<svg fill="black" height="24" viewBox="0 0 24 24" width="24" xmlns="http://www.w3.org/2000/svg"><path d="M7 10l5 5 5-5z"/></svg>');
  background-repeat: no-repeat;
  background-position: right 1rem center;
  background-size: 1rem;
}
#error-message {
  color: #a80000;
  background-color: #fdecea;
  border: 1px solid #f5c2c0;
  padding: 1rem;
  border-radius: 10px;
  margin-bottom: 1rem;
  display: none;
  font-size: 0.9rem;
}

.input-error {
  border: 2px solid #ea0b82 !important;
  background-color: #fff0f5 !important;
}

  </style>
</head>
<body>
  <div class="main-wrapper">
    <div class="container" id="calcBox">
      <div class="card">
        <h1>Psychological Safety Deficit Calculator</h1>
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
        <h2>Average Annual Salary per Race Group (ZAR)</h2>
        <div class="grid">
          <div><label>Black</label><input type="number" id="blackSalary" /></div>
          <div><label>White</label><input type="number" id="whiteSalary" /></div>
          <div><label>Coloured</label><input type="number" id="colouredSalary" /></div>
          <div><label>Indian/Asian</label><input type="number" id="indianasianSalary" /></div>
        </div>
      </div>
      <div class="card purple-card">
<h2>
  How would you rate your organisation's current level of psychological safety?
  <span class="tooltip" data-tooltip="This is a self-assessment of your organisation’s culture of psychological safety. You can base it on employee feedback, surveys, exit interviews, or observed behaviours — whatever best reflects your current reality.">
    <img src="Untitled design.svg" alt="info icon" />
  </span>
</h2>        <select id="cultureRating">
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
      <h2>Estimated Annual Cost of Psych Safety Neglect</h2>
<div class="result-line">
  <span>
    Excess Resignations:
    <span class="tooltip" data-tooltip="Number of unnecessary staff churn owing to lower levels of psychological safety calculated at X% of XXX when XXX level of psychological safety selected.">
      <img src="Untitled design.svg" alt="info icon" />
    </span>
  </span>
  <span id="resignations">RXXX</span>
  </div>  
<div class="result-line">
  <span>
    Cost of Excess Staff Turnover:
    <span class="tooltip" data-tooltip="Cost of staff churn calculated at 50% of average annual salary per demographic group.">
      <img src="Untitled design.svg" alt="info icon" />
    </span>
  </span>
  <span id="turnover">RXXX</span>
</div>      
<div class="result-line">
  <span>
    Absenteeism:
    <span class="tooltip" data-tooltip="Based on excess leave days multiplied by daily salary and adjusted by 88% to reflect average working time.">
      <img src="Untitled design.svg" alt="info icon" />
    </span>
  </span>
  <span id="absenteeism">RXXX</span>
</div>
<div class="result-line">
  <span>
    Presenteeism:
    <span class="tooltip" data-tooltip="Calculated using average presenteeism rates (%) applied to total salary cost per demographic group.">
      <img src="Untitled design.svg" alt="info icon" />
    </span>
  </span>
  <span id="presenteeism">RXXX</span>
</div>
<div class="result-line total-divider"><span>Total Estimated Cost</span><span id="total">RXXX</span></div>
      <div class="result-buttons">
<button class="primary" onclick="openEmailModal()">Email Me This Report</button>
        <button class="primary">Enquire about our Solution</button>
<button class="secondary" onclick="resetCalculator()">Reset Calculator</button>
      </div>
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
          absenteeismDays: {
            black: 2, white: 0.5, coloured: 1, indianasian: 1
          },
          presenteeismRates: {
            black: 0.15, white: 0.0375, coloured: 0.09375, indianasian: 0.09375
          }
        },
        medium: {
          turnoverRates: {
            black: { men: 0.03, women: 0.04 },
            white: { men: 0.005, women: 0.01 },
            coloured: { men: 0.01, women: 0.02 },
            indianasian: { men: 0.01, women: 0.02 }
          },
          absenteeismDays: {
            black: 1, white: 0.25, coloured: 0.5, indianasian: 0.5
          },
          presenteeismRates: {
            black: 0.075, white: 0.015, coloured: 0.045, indianasian: 0.045
          }
        },
        high: {
          turnoverRates: {
            black: { men: 0.01, women: 0.02 },
            white: { men: 0.0025, women: 0.005 },
            coloured: { men: 0.005, women: 0.01 },
            indianasian: { men: 0.005, women: 0.01 }
          },
          absenteeismDays: {
            black: 0.5, white: 0.1, coloured: 0.25, indianasian: 0.25
          },
          presenteeismRates: {
            black: 0.0375, white: 0.0075, coloured: 0.01875, indianasian: 0.01875
          }
        }
      };
      return rates[level];
    }

    function calculateCosts() {
      const total = parseFloat(document.getElementById('totalStaff').value);
      const getPct = id => parseFloat(document.getElementById(id).value || 0) / 100;
      const getVal = id => parseFloat(document.getElementById(id).value || 0);
      const genderSplit = {
        men: getPct('menPct'),
        women: getPct('womenPct')
      };
      const culture = document.getElementById('cultureRating').value;
      const { turnoverRates, absenteeismDays, presenteeismRates } = getRates(culture);
      const raceGroups = {
        black: { pct: getPct('blackPct'), salary: getVal('blackSalary') },
        white: { pct: getPct('whitePct'), salary: getVal('whiteSalary') },
        coloured: { pct: getPct('colouredPct'), salary: getVal('colouredSalary') },
        indianasian: { pct: getPct('indianasianPct'), salary: getVal('indianasianSalary') }
      };
      let turnoverCost = 0, absenteeismCost = 0, presenteeismCost = 0, totalExits = 0;

      for (const [race, group] of Object.entries(raceGroups)) {
        const headcount = total * group.pct;
        const maleHeadcount = headcount * genderSplit.men;
        const femaleHeadcount = headcount * genderSplit.women;
        const exits = (maleHeadcount * turnoverRates[race].men) + (femaleHeadcount * turnoverRates[race].women);
        totalExits += exits;
        turnoverCost += exits * (0.5 * group.salary);
        absenteeismCost += absenteeismDays[race] * (group.salary / 220) * headcount * 0.88;
        presenteeismCost += headcount * group.salary * presenteeismRates[race];
      }
      const raceTotal = getPct('blackPct') + getPct('whitePct') + getPct('colouredPct') + getPct('indianasianPct');
const genderTotal = getPct('menPct') + getPct('womenPct');
      
if (!culture) {
  document.getElementById('cultureRating').classList.add('input-error');
  const errorBox = document.getElementById('error-message');
  errorBox.textContent = 'Please select your organisation\'s level of psychological safety.';
  errorBox.style.display = 'block';
  return;
}
// Clear previous errors
document.getElementById('error-message').style.display = 'none';
document.getElementById('error-message').textContent = '';
document.querySelectorAll('.input-error').forEach(el => el.classList.remove('input-error'));

if (Math.abs(raceTotal - 1) > 0.01 || Math.abs(genderTotal - 1) > 0.01) {
  let errorMessage = 'Please ensure that ';
  if (Math.abs(raceTotal - 1) > 0.01) {
    errorMessage += 'race ';
    ['blackPct', 'whitePct', 'colouredPct', 'indianasianPct'].forEach(id =>
      document.getElementById(id).classList.add('input-error')
    );
  }
  if (Math.abs(genderTotal - 1) > 0.01) {
    errorMessage += (Math.abs(raceTotal - 1) > 0.01 ? 'and gender ' : 'gender ');
    ['womenPct', 'menPct'].forEach(id =>
      document.getElementById(id).classList.add('input-error')
    );
  }
  errorMessage += 'percentages each add up to exactly 100%.';
  const errorBox = document.getElementById('error-message');
  errorBox.textContent = errorMessage;
  errorBox.style.display = 'block';
  return;
}
      const totalCost = turnoverCost + absenteeismCost + presenteeismCost;

      document.getElementById('resignations').textContent = totalExits.toFixed(1);
      document.getElementById('turnover').textContent = 'R ' + Math.round(turnoverCost).toLocaleString();
      document.getElementById('absenteeism').textContent = 'R ' + Math.round(absenteeismCost).toLocaleString();
      document.getElementById('presenteeism').textContent = 'R ' + Math.round(presenteeismCost).toLocaleString();
      document.getElementById('total').textContent = 'R ' + Math.round(totalCost).toLocaleString();

      document.getElementById('calcBox').classList.add('shrink');
      document.getElementById('resultBox').style.display = 'block';
    }
          function openEmailModal() {
  document.getElementById('emailModal').style.display = 'flex';

  // Populate hidden field with result
  const totalCost = document.getElementById('total').textContent;
  document.getElementById('hiddenTotalCost').value = totalCost;
}

function closeEmailModal() {
  document.getElementById('emailModal').style.display = 'none';
}
    function resetCalculator() {
  // Clear all inputs
  const inputs = document.querySelectorAll('input, select');
  inputs.forEach(input => {
    if (input.tagName === 'SELECT') {
      input.selectedIndex = 0;
    } else {
      input.value = '';
    }
  });

  // Hide the results box
  document.getElementById('resultBox').style.display = 'none';

  // Expand calculator box
  document.getElementById('calcBox').classList.remove('shrink');
}

    // Show Enquiry Modal
document.querySelector('.result-buttons .primary:nth-child(2)').addEventListener('click', function () {
  document.getElementById('enquiryModal').style.display = 'flex';
});

// Close Modal
function closeModal() {
  document.getElementById('enquiryModal').style.display = 'none';
}
  </script>
  <div id="enquiryModal" style="display:none; position:fixed; top:0; left:0; width:100%; height:100%; background-color:rgba(0,0,0,0.5); z-index:1000; justify-content:center; align-items:center;">
  <div style="background:white; padding:2rem; border-radius:20px; max-width:500px; width:90%; position:relative; font-family: 'Montserrat', sans-serif;">
    <button onclick="closeModal()" style="position:absolute; top:10px; right:15px; border:none; background:none; font-size:1.5rem; cursor:pointer;">&times;</button>
    <h2 style="margin-top:0;">Enquire About Our Solution</h2>
    <form action="https://formspree.io/f/movlkdbj" method="POST">
      <label for="name">Name</label>
      <input type="text" name="name" required style="width:100%; padding:0.75rem; margin-bottom:1rem; border-radius:30px; border:1px solid #ccc;">

      <label for="email">Email</label>
      <input type="email" name="email" required style="width:100%; padding:0.75rem; margin-bottom:1rem; border-radius:30px; border:1px solid #ccc;">

      <label for="message">Message</label>
      <textarea name="message" rows="4" required style="width:100%; padding:0.75rem; margin-bottom:1rem; border-radius:20px; border:1px solid #ccc;"></textarea>

      <button type="submit" style="background-color:#5700ff; color:white; border:none; padding:1rem 2rem; border-radius:999px; font-weight:500; cursor:pointer;">Send</button>
    </form>
  </div>
</div>
<div id="emailModal" style="display:none; position:fixed; top:0; left:0; width:100%; height:100%; background-color:rgba(0,0,0,0.5); z-index:1000; justify-content:center; align-items:center;">
  <div style="background:white; padding:2rem; border-radius:20px; max-width:500px; width:90%; position:relative; font-family: 'Montserrat', sans-serif;">
    <button onclick="closeEmailModal()" style="position:absolute; top:10px; right:15px; border:none; background:none; font-size:1.5rem; cursor:pointer;">&times;</button>
    <h2 style="margin-top:0;">Get Your Report by Email</h2>
    <form action="https://formspree.io/f/movlkdbj" method="POST">
      <label for="firstName">First Name</label>
      <input type="text" name="firstName" required style="width:100%; padding:0.75rem; margin-bottom:1rem; border-radius:30px; border:1px solid #ccc;" />

      <label for="lastName">Last Name</label>
      <input type="text" name="lastName" required style="width:100%; padding:0.75rem; margin-bottom:1rem; border-radius:30px; border:1px solid #ccc;" />

      <label for="email">Email Address</label>
      <input type="email" name="email" required style="width:100%; padding:0.75rem; margin-bottom:1rem; border-radius:30px; border:1px solid #ccc;" />

      <input type="hidden" name="totalCost" id="hiddenTotalCost" />

      <input type="hidden" name="_cc" value="roy@runtothemonster.com" />

      <button type="submit" style="background-color:#5700ff; color:white; border:none; padding:1rem 2rem; border-radius:999px; font-weight:500; cursor:pointer;">Send Report</button>
    </form>
  </div>
</div>
<script>
  document.querySelectorAll('input, select').forEach(input => {
    input.addEventListener('input', () => {
input.classList.remove('input-error');
if (input.id === 'cultureRating') {
  input.style.backgroundColor = '#fff'; // Optional: reset dropdown colour if modified
}      document.getElementById('error-message').style.display = 'none';
    });
  });
</script>
</body>
</html>
