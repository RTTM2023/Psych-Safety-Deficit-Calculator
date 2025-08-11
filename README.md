<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Psychological Safety Cost Calculator</title>
  <style>
    :root{
      --violet:#5700ff;--violet-dark:#5200ff;--pink:#ea0b82;--lav:#E3C8F7;--lav-soft:#e4c8f7;--error:#a80000;--error-bg:#fdecea;--error-br:#f5c2c0;
    }
    *{box-sizing:border-box}
    body{font-family:'Montserrat',system-ui,-apple-system,Segoe UI,Roboto,Arial,sans-serif;margin:0;background:transparent;color:#111}

    .main-wrapper{display:flex;gap:1rem;align-items:flex-start;padding:2rem;max-width:1200px;margin:0 auto}
    @media (max-width: 980px){
      .main-wrapper{flex-direction:column}
      .container{width:100%}
      .container.shrink{width:100%}
      .result-wrapper{width:100%}
    }

    .container{width:580px;flex-shrink:0;transition:width .3s ease}
    .container.shrink{width:480px}

    .result-wrapper{flex:1;background:var(--violet);color:#fff;min-height:300px;align-self:flex-start;display:none;border-radius:20px;padding:2rem}

    .card,.subcard{border-radius:24px;padding:2rem;margin-bottom:0;border:1px solid var(--lav)}
    .card{background:#fff}
    .purple-card{background:var(--lav-soft)}

    h1{font-size:1.75rem;font-weight:700;margin:0 0 .5rem}
    h2{font-size:.8rem;font-weight:700;margin:1rem 0 .5rem}

    label{font-weight:500;font-size:.9rem;display:block;margin-bottom:.25rem}

    input,select{width:100%;padding:.75rem;margin-bottom:0;border:none;border-radius:30px;font:inherit}
    .purple-card input#totalStaff,
    .purple-card input#womenPct,
    .purple-card input#menPct,
    .purple-card input#blackPct,
    .purple-card input#whitePct,
    .purple-card input#colouredPct,
    .purple-card input#indianasianPct,
    .purple-card select#cultureRating{background:#fff}
    .card input,.card select{background:var(--lav)}

    .grid{display:grid;grid-template-columns:repeat(2,1fr);gap:1rem}

    .pink-line{height:2px;background:var(--pink);width:50%;margin:1.5rem 0}

    button{width:100%;background:var(--violet-dark);color:#fff;font-weight:700;padding:1rem;border:none;border-radius:20px;font-size:1rem;cursor:pointer}

    .result-wrapper h2{font-size:1.2rem;font-weight:700;margin:0 0 1rem;border-bottom:2px solid #fff;padding-bottom:.5rem}

    .result-line{display:flex;justify-content:space-between;margin:.4rem 0;font-size:.95rem;gap:1rem}
    .result-line.total-divider{margin:1.5rem 0 .5rem;border-top:2px dotted #fff;border-bottom:2px dotted #fff;padding:1rem 0;font-size:1.05rem;font-weight:800}

    .result-buttons{margin-top:2rem;display:flex;flex-direction:column;gap:1rem}
    .result-buttons .primary{background:#fff;color:var(--violet);border:2px dotted var(--pink);font-weight:600;padding:1rem 1.5rem;border-radius:999px;font-size:1rem;cursor:pointer}
    .result-buttons .secondary{background:var(--pink);color:#fff;border:none;font-weight:600;padding:1rem 1.5rem;border-radius:999px;font-size:1rem;cursor:pointer}

    #error-message{color:var(--error);background:var(--error-bg);border:1px solid var(--error-br);padding:1rem;border-radius:10px;margin:1rem 0;display:none;font-size:.9rem}
    .input-error{border:2px solid var(--pink)!important;background:#fff0f5!important}

    /* Tooltip */
    .tooltip{position:relative;display:inline-block;vertical-align:super;margin-left:2px;top:-.2em}
    .tooltip img{width:14px;height:14px;display:inline;background:transparent;vertical-align:middle;line-height:1}
    .tooltip:hover::after{content:attr(data-tooltip);position:absolute;background:rgba(0,0,0,.85);color:#fff;padding:.6rem .8rem;border-radius:6px;top:120%;left:50%;transform:translateX(-50%);display:block;max-width:260px;width:max-content;min-width:140px;white-space:normal;font-size:.8rem;z-index:999;text-align:left}

    .totals-chip{display:inline-flex;align-items:center;gap:.4rem;background:#fff;border:1px dashed var(--pink);border-radius:999px;padding:.25rem .6rem;font-size:.8rem;font-weight:600}
  </style>
</head>
<body>
  <div class="main-wrapper" id="appRoot">
    <div class="container" id="calcBox" aria-describedby="error-message">
      <div class="card">
        <h1>How Much Does Psychological (Un)Safety Cost Us?</h1>
      </div>

      <div class="card purple-card">
        <h2>Total Staff Complement</h2>
        <input type="text" id="totalStaff" placeholder="e.g. 1000" inputmode="numeric" pattern="[0-9]*" aria-label="Total Staff Complement" />
        <div class="pink-line"></div>

        <div class="grid" aria-label="Race composition in percent">
          <div><label for="blackPct">% Black</label><input type="number" id="blackPct" min="0" max="100" step="0.1" placeholder="e.g. 70" /></div>
          <div><label for="whitePct">% White</label><input type="number" id="whitePct" min="0" max="100" step="0.1" placeholder="e.g. 20" /></div>
          <div><label for="colouredPct">% Coloured</label><input type="number" id="colouredPct" min="0" max="100" step="0.1" placeholder="e.g. 7" /></div>
          <div><label for="indianasianPct">% Indian/Asian</label><input type="number" id="indianasianPct" min="0" max="100" step="0.1" placeholder="e.g. 3" /></div>
        </div>
        <div style="margin-top:.6rem"><span class="totals-chip">Race total: <span id="raceTotal">0%</span></span></div>

        <div class="pink-line"></div>
        <div class="grid" aria-label="Gender composition in percent">
          <div><label for="womenPct">% Women</label><input type="number" id="womenPct" min="0" max="100" step="0.1" placeholder="e.g. 45" /></div>
          <div><label for="menPct">% Men</label><input type="number" id="menPct" min="0" max="100" step="0.1" placeholder="e.g. 55" /></div>
        </div>
        <div style="margin-top:.6rem"><span class="totals-chip">Gender total: <span id="genderTotal">0%</span></span></div>
      </div>

      <div class="card">
        <h2>Average Annual Salary</h2>
        <input type="text" id="avgSalary" placeholder="e.g. 350,000" inputmode="numeric" aria-label="Average annual salary (numbers only)" />
        <div id="error-message" role="alert" aria-live="polite"></div>
      </div>

      <div class="card purple-card">
        <h2>
          How would you rate your organisation's current level of psychological safety?
          <span class="tooltip" role="note" aria-label="More information" data-tooltip="This is a self-assessment of your organisation’s culture of psychological safety. You can base it on employee feedback, surveys, exit interviews, or observed behaviours — whatever best reflects your current reality.">
            <img src="Untitled design.svg" alt="Info" />
          </span>
        </h2>
        <select id="cultureRating" aria-label="Psychological safety rating">
          <option value="" disabled selected hidden>Select</option>
          <option value="low">Low</option>
          <option value="medium">Medium</option>
          <option value="high">High</option>
        </select>
      </div>

      <button class="calculate" id="calcBtn">Calculate</button>
    </div>

    <div class="result-wrapper" id="resultBox" aria-live="polite">
      <h2>Estimated Financial Losses due to Psychological Unsafety per Year</h2>

      <div class="result-line">
        <span>
          Excess Resignations:
          <span class="tooltip" data-tooltip="Number of employees leaving over-and-above normal company churn due to psychological safety."><img src="Untitled design.svg" alt="Info" /></span>
        </span>
        <span id="resignations">—</span>
      </div>

      <div class="result-line">
        <span>
          Cost of Excess Staff Turnover:
          <span class="tooltip" data-tooltip="HR resources, recruitment costs and lower productivity during onboarding of new recruits to fill vacancies left by excess resignations."><img src="Untitled design.svg" alt="Info" /></span>
        </span>
        <span id="turnover">—</span>
      </div>

      <div class="result-line">
        <span>
          Excess Sick Days/Absenteeism:
          <span class="tooltip" data-tooltip="Days and hours paid for, but lost due to mental stress and related illness."><img src="Untitled design.svg" alt="Info" /></span>
        </span>
        <span id="absenteeism">—</span>
      </div>

      <div class="result-line">
        <span>
          On-the-job Underperformance:
          <span class="tooltip" data-tooltip="Days and weeks paid for, but lost due to disengagement and low productivity. Poor collaboration in diverse teams leads to time lost on resolving conflicts or repeating work due to miscommunication instead of productive activity."><img src="Untitled design.svg" alt="Info" /></span>
        </span>
        <span id="presenteeism">—</span>
      </div>

      <div class="result-line total-divider"><span>Total Estimated Cost</span><span id="total">—</span></div>

      <div class="result-buttons">
        <button class="primary" id="emailBtn">Email Me This Report</button>
        <button class="primary" id="enquireBtn">Enquire about our Solution</button>
        <button class="secondary" id="resetBtn">Reset Calculator</button>
      </div>
    </div>
  </div>

  <!-- Enquiry Modal -->
  <div id="enquiryModal" style="display:none;position:fixed;inset:0;background:rgba(0,0,0,.5);z-index:1000;justify-content:center;align-items:center;">
    <div class="modal-content" style="background:#fff;padding:2rem;border-radius:20px;max-width:500px;width:90%;position:relative;font-family:'Montserrat',sans-serif;">
      <button class="modal-close" onclick="closeModal()" aria-label="Close" style="position:absolute;top:10px;right:12px;border:none;background:none;font-size:28px;color:var(--violet);cursor:pointer;line-height:1;padding:0">&times;</button>
      <h2 style="margin-top:0">Enquire About Our Solution</h2>
      <form id="enquiryForm" action="https://formspree.io/f/movlkdbj" method="POST">
        <input type="hidden" name="_subject" value="Psych Safety Calculator - Enquiry about Solution">
        <label for="name">Name</label>
        <input type="text" name="name" required style="width:100%;padding:.75rem;margin-bottom:1rem;border-radius:30px;border:1px solid #ccc">
        <label for="email">Email</label>
        <input type="email" name="email" required style="width:100%;padding:.75rem;margin-bottom:1rem;border-radius:30px;border:1px solid #ccc">
        <label for="message">Message</label>
        <textarea name="message" rows="4" required style="width:100%;padding:.75rem;margin-bottom:1rem;border-radius:20px;border:1px solid #ccc"></textarea>
        <button type="submit" style="background:var(--violet);color:#fff;border:none;padding:1rem 2rem;border-radius:999px;font-weight:600;cursor:pointer">Send</button>
      </form>
    </div>
  </div>

  <!-- Email Modal -->
  <div id="emailModal" style="display:none;position:fixed;inset:0;background:rgba(0,0,0,.5);z-index:1000;justify-content:center;align-items:center;">
    <div class="modal-content" style="background:#fff;padding:2rem;border-radius:20px;max-width:500px;width:90%;position:relative;font-family:'Montserrat',sans-serif;">
      <button class="modal-close" onclick="closeEmailModal()" aria-label="Close" style="position:absolute;top:10px;right:12px;border:none;background:none;font-size:28px;color:var(--violet);cursor:pointer;line-height:1;padding:0">&times;</button>
      <h2 style="margin-top:0">Get Your Report by Email</h2>
      <form id="emailForm" action="https://formspree.io/f/movlkdbj" method="POST">
        <input type="hidden" name="_subject" value="Psych Safety Calculator - Request Report">
        <label for="firstName">First Name</label>
        <input type="text" name="firstName" required style="width:100%;padding:.75rem;margin-bottom:1rem;border-radius:30px;border:1px solid #ccc" />
        <label for="lastName">Last Name</label>
        <input type="text" name="lastName" required style="width:100%;padding:.75rem;margin-bottom:1rem;border-radius:30px;border:1px solid #ccc" />
        <label for="email">Email Address</label>
        <input type="email" name="email" required style="width:100%;padding:.75rem;margin-bottom:1rem;border-radius:30px;border:1px solid #ccc" />
        <input type="hidden" name="totalCost" id="hiddenTotalCost" />
        <button type="submit" style="background:var(--violet);color:#fff;border:none;padding:1rem 2rem;border-radius:999px;font-weight:600;cursor:pointer">Send Report</button>
      </form>
    </div>
  </div>

<script>
  // ---------- Utility ----------
  const ZAR = new Intl.NumberFormat('en-ZA',{ style:'currency', currency:'ZAR', maximumFractionDigits:0 });
  const pct = n => (n*100).toFixed(1).replace(/\.0$/,'') + '%';
  const num = (s) => {
    // Accept digits, spaces, commas, periods; strip non-digits
    if (typeof s !== 'string') return NaN;
    const cleaned = s.replace(/[^0-9]/g,'');
    return cleaned ? parseInt(cleaned,10) : NaN;
  };

  // ---------- Rates ----------
  function getRates(level){
    const rates={
      low:{
        turnoverRates:{ black:{men:.07,women:.08}, white:{men:.01,women:.015}, coloured:{men:.03,women:.04}, indianasian:{men:.03,women:.04} },
        absenteeismDays:{ black:2, white:.5, coloured:1, indianasian:1 },
        presenteeismRates:{ black:.15, white:.0375, coloured:.09375, indianasian:.09375 }
      },
      medium:{
        turnoverRates:{ black:{men:.03,women:.04}, white:{men:.005,women:.01}, coloured:{men:.01,women:.02}, indianasian:{men:.01,women:.02} },
        absenteeismDays:{ black:1, white:.25, coloured:.5, indianasian:.5 },
        presenteeismRates:{ black:.075, white:.015, coloured:.045, indianasian:.045 }
      },
      high:{
        turnoverRates:{ black:{men:.01,women:.02}, white:{men:.0025,women:.005}, coloured:{men:.005,women:.01}, indianasian:{men:.005,women:.01} },
        absenteeismDays:{ black:.5, white:.1, coloured:.25, indianasian:.25 },
        presenteeismRates:{ black:.0375, white:.0075, coloured:.01875, indianasian:.01875 }
      }
    };
    return rates[level];
  }

  // ---------- Calculation ----------
  function calculateCosts(){
    const errorBox=document.getElementById('error-message');
    errorBox.textContent=''; errorBox.style.display='none';
    document.querySelectorAll('.input-error').forEach(el=>el.classList.remove('input-error'));

    const cultureSel=document.getElementById('cultureRating');
    const culture=cultureSel.value;

    // Salary and headcount (allow commas/spaces)
    const salaryEl=document.getElementById('avgSalary');
    const salary=num(salaryEl.value.trim());

    const totalStaffEl=document.getElementById('totalStaff');
    const total=num(totalStaffEl.value.trim());

    const getPct=(id)=>{
      const v=parseFloat(document.getElementById(id).value || '');
      return isNaN(v)?NaN:v/100;
    };

    const raceIds=['blackPct','whitePct','colouredPct','indianasianPct'];
    const genderIds=['womenPct','menPct'];

    const raceVals=raceIds.map(id=>parseFloat(document.getElementById(id).value||'0'));
    const genderVals=genderIds.map(id=>parseFloat(document.getElementById(id).value||'0'));

    const raceTotal=raceVals.reduce((a,b)=>a+(isNaN(b)?0:b),0);
    const genderTotal=genderVals.reduce((a,b)=>a+(isNaN(b)?0:b),0);

    // Live chips
    document.getElementById('raceTotal').textContent=pct((isFinite(raceTotal)?raceTotal:0)/100);
    document.getElementById('genderTotal').textContent=pct((isFinite(genderTotal)?genderTotal:0)/100);

    let hasError=false; let msg=[]; let firstError=null;

    if(!culture){
      cultureSel.classList.add('input-error'); msg.push("Please select your organisation's level of psychological safety."); hasError=true; firstError=firstError||cultureSel;
    }

    if(!Number.isFinite(total) || total<=0){
      totalStaffEl.classList.add('input-error'); msg.push('Total Staff must be a positive number.'); hasError=true; firstError=firstError||totalStaffEl;
    }
    if(!Number.isFinite(salary) || salary<=0){
      salaryEl.classList.add('input-error'); msg.push('Average Annual Salary must be a positive number (you can include commas).'); hasError=true; firstError=firstError||salaryEl;
    }

    // Range checks 0..100
    [...raceIds, ...genderIds].forEach(id=>{
      const el=document.getElementById(id); const v=parseFloat(el.value);
      if(isNaN(v) || v<0 || v>100){
        el.classList.add('input-error'); hasError=true; firstError=firstError||el;
      }
    });

    // Sum checks (±1%)
    if(Math.abs(raceTotal-100)>1){
      raceIds.forEach(id=>document.getElementById(id).classList.add('input-error'));
      msg.push('Race percentages must add up to 100%.'); hasError=true;
    }
    if(Math.abs(genderTotal-100)>1){
      genderIds.forEach(id=>document.getElementById(id).classList.add('input-error'));
      msg.push('Gender percentages must add up to 100%.'); hasError=true;
    }

    if(hasError){
      errorBox.textContent=msg.join('\n'); errorBox.style.display='block';
      if(firstError) firstError.focus();
      return;
    }

    const { turnoverRates, absenteeismDays, presenteeismRates }=getRates(culture);

    const raceGroups={
      black:{ pct:getPct('blackPct'), salary },
      white:{ pct:getPct('whitePct'), salary },
      coloured:{ pct:getPct('colouredPct'), salary },
      indianasian:{ pct:getPct('indianasianPct'), salary }
    };
    const genderSplit={ men:getPct('menPct'), women:getPct('womenPct') };

    let turnoverCost=0, absenteeismCost=0, presenteeismCost=0, totalExits=0;

    for(const [race,group] of Object.entries(raceGroups)){
      const headcount= total * group.pct;
      const maleHeadcount= headcount * genderSplit.men;
      const femaleHeadcount= headcount * genderSplit.women;
      const exits= (maleHeadcount * turnoverRates[race].men) + (femaleHeadcount * turnoverRates[race].women);
      totalExits += exits;

      // Cost components
      turnoverCost += exits * (0.5 * group.salary); // 50% of salary per exit
      absenteeismCost += absenteeismDays[race] * (group.salary / 220) * headcount * 0.88; // 220 working days; 88% paid time factor
      presenteeismCost += headcount * group.salary * presenteeismRates[race];
    }

    const totalCost = turnoverCost + absenteeismCost + presenteeismCost;

    // Display
    document.getElementById('resignations').textContent = Math.floor(totalExits).toLocaleString('en-ZA');
    document.getElementById('turnover').textContent     = ZAR.format(Math.round(turnoverCost));
    document.getElementById('absenteeism').textContent  = ZAR.format(Math.round(absenteeismCost));
    document.getElementById('presenteeism').textContent = ZAR.format(Math.round(presenteeismCost));
    document.getElementById('total').textContent        = ZAR.format(Math.round(totalCost));

    // Reveal + scroll
    document.getElementById('calcBox').classList.add('shrink');
    const rb=document.getElementById('resultBox');
    rb.style.display='block';
    rb.scrollIntoView({behavior:'smooth',block:'start'});

    // Pass value to email modal
    document.getElementById('hiddenTotalCost').value = ZAR.format(Math.round(totalCost));
  }

  // ---------- Modal helpers ----------
  function openEmailModal(){ document.getElementById('emailModal').style.display='flex'; }
  function closeEmailModal(){ document.getElementById('emailModal').style.display='none'; }
  function closeModal(){ document.getElementById('enquiryModal').style.display='none'; }

  function resetCalculator(){
    document.querySelectorAll('input, select').forEach(input=>{
      if(input.tagName==='SELECT') input.selectedIndex=0; else input.value='';
    });
    document.getElementById('resultBox').style.display='none';
    document.getElementById('calcBox').classList.remove('shrink');
    const errorBox=document.getElementById('error-message');
    errorBox.textContent=''; errorBox.style.display='none';
    document.querySelectorAll('.input-error').forEach(el=>el.classList.remove('input-error'));
    document.getElementById('raceTotal').textContent='0%';
    document.getElementById('genderTotal').textContent='0%';
  }

  // ---------- Wiring ----------
  document.addEventListener('DOMContentLoaded',()=>{
    // Buttons
    document.getElementById('calcBtn').addEventListener('click',calculateCosts);
    document.getElementById('emailBtn').addEventListener('click',openEmailModal);
    document.getElementById('enquireBtn').addEventListener('click',()=>{ document.getElementById('enquiryModal').style.display='flex'; });
    document.getElementById('resetBtn').addEventListener('click',resetCalculator);

    // Close modals when clicking backdrop
    ['enquiryModal','emailModal'].forEach(id=>{
      const modal=document.getElementById(id);
      modal.addEventListener('click',e=>{ if(e.target===modal) modal.style.display='none'; });
    });

    // ESC to close modals
    document.addEventListener('keydown',e=>{
      if(e.key==='Escape'){
        const enquiry=document.getElementById('enquiryModal');
        const email=document.getElementById('emailModal');
        if(enquiry && enquiry.style.display==='flex') enquiry.style.display='none';
        if(email && email.style.display==='flex') email.style.display='none';
      }
    });

    // Remove red highlight & hide error message as user types
    document.querySelectorAll('input, select').forEach(input=>{
      input.addEventListener('input',()=>{
        input.classList.remove('input-error');
        const box=document.getElementById('error-message');
        if(box.textContent) box.style.display='none';
        // Update live total chips for percentages
        const idsRace=['blackPct','whitePct','colouredPct','indianasianPct'];
        const idsGen=['womenPct','menPct'];
        const rTot=idsRace.reduce((a,id)=>a+(parseFloat(document.getElementById(id).value)||0),0);
        const gTot=idsGen.reduce((a,id)=>a+(parseFloat(document.getElementById(id).value)||0),0);
        document.getElementById('raceTotal').textContent=pct(rTot/100);
        document.getElementById('genderTotal').textContent=pct(gTot/100);
      });
    });
  });
</script>

</body>
</html>
