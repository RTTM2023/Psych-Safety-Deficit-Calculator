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

    #appTitle { display: block !important; }

    section.page-header,
    header.page-header,
    .page-header .project-name,
    .page-header .project-tagline,
    header[role="banner"] {
      display: none !important;
    }

    .main-wrapper {
      display: flex;
      gap: 2rem; /* Increased gap for better separation */
      align-items: flex-start;
      padding: 2rem;
      max-width: 1400px; /* Increased from 1200px to give results more room */
      margin: 0 auto;
    }

    .container {
      width: 48%; /* Changed from fixed 580px to percentage */
      min-width: 450px; /* Ensures it doesn't get too small */
      flex-shrink: 0;
      transition: all 0.3s ease;
    }

    /* When results box is shown, the input box stays a professional size */
    .container.shrink { width: 40%; }

    .result-wrapper {
      flex: 1; /* Automatically takes up the remaining 60% of the 1400px */
      background-color: #5700ff;
      color: white;
      min-height: 300px;
      align-self: flex-start;
      display: none;
      border-radius: 20px;
      padding: 2.5rem; /* Slightly more padding for desktop elegance */
      box-sizing: border-box;
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
      margin-bottom: 1.5rem;
      border-bottom: 2px solid white;
      padding-bottom: 0.5rem;
    }

    .result-line { display: flex; justify-content: space-between; margin: 0.6rem 0; font-size: 0.95rem; }

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

    #enquiryModal.show { display: flex !important; }

    .tooltip { position: relative; display: inline-block; vertical-align: super; margin-left: 2px; top: -0.2em; }
    .tooltip img { width: 14px; height: 14px; display: inline; margin: 0; padding: 0; background-color: transparent; vertical-align: middle; line-height: 1; }
    .tooltip:hover::after {
      content: attr(data-tooltip);
      position: absolute;
      background: rgba(0,0,0,0.85);
      color: #fff; padding: 0.6rem 0.8rem; border-radius: 5px; top: 120%; left: 50%;
      transform: translateX(-50%); display: block; max-width: 240px; width: max-content; min-width: 120px;
      white-space: normal; font-size: 0.8rem; z-index: 999; text-align: left;
    }

    .container h2 { font-size: 1.2rem; font-weight: 700; margin-bottom: 1rem; padding-bottom: 0.5rem; border-bottom: none; }
    h2 > a { display: none; }
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

    .modal-content { position: relative; }
    .modal-close {
      position: absolute; top: 10px; right: 12px; border: none; background: none;
      font-size: 28px; color: #5700ff; cursor: pointer; line-height: 1; padding: 0;
    }
    .modal-close:hover, .modal-close:focus { opacity: 0.8; outline: none; }

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

  .result-line {
    display: grid;                 
    grid-template-columns: 1fr auto;
    align-items: baseline;
    column-gap: 0.5rem;
  }

  .result-line > span:last-child {
    white-space: nowrap;           
    text-align: right;
    font-variant-numeric: tabular-nums; 
  }

  .result-line.total-divider {
    display: grid;
    grid-template-columns: 1fr auto;
  }

  .tooltip img { width: 12px; height: 12px; }

  /* Reverting to full width for mobile */
  .container, .container.shrink { width: 100%; min-width: 100%; }

  .result-wrapper {
    width: 100%;
    margin-top: 0.75rem;
    border-radius: 16px;
    padding: 1.5rem;
    min-height: auto;
  }

  .card, .subcard { padding: 1rem; border-radius: 16px; }
  h1 { font-size: 1.25rem; margin-bottom: 0.25rem; }
  h2 { font-size: 1rem; margin: 0.75rem 0 0.5rem; }

  .grid { grid-template-columns: 1fr; gap: 0.75rem; }

  input, select, button, textarea {
    font-size: 16px;
    line-height: 1.25;
    padding: 0.9rem;
    border-radius: 16px;
  }

  .result-line { font-size: 0.95rem; }
  .result-buttons { gap: 0.75rem; }
  .result-buttons .primary,
  .result-buttons .secondary {
    width: 100%;
    padding: 0.9rem 1rem;
    border-radius: 999px;
    font-size: 1rem;
  }

  .tooltip:hover::after {
    max-width: 80vw;
    left: 50%;
    transform: translateX(-50%);
    top: 110%;
    font-size: 0.9rem;
  }

  #enquiryModal .modal-content,
  #emailModal .modal-content {
    width: 92%;
    max-width: 520px;
    max-height: 85vh;
    overflow: auto;
    padding: 1.25rem;
    border-radius: 16px;
  }
  .modal-close { font-size: 32px; right: 10px; top: 8px; }
}

@media (max-width: 360px) {
  h1 { font-size: 1.1rem; }
  .result-line { font-size: 0.9rem; }
}
  </style>
</head>
