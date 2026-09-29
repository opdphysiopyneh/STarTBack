# STarTBack

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>PYNEH Physiotherapy Department - STarT Back Screening Tool</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0, shrink-to-fit=no, viewport-fit=cover">
  <style>
    :root {
      --primary: #007AFF;
      --primary-active: #0056b3;
      --bg: #F2F2F7;
      --card-bg: #FFFFFF;
      --text-main: #000000;
      --text-muted: #6C6C70;
      --border: #D1D1D6;
      --selected-bg: #E3F0FF;
      --error-border: #FF3B30;
      --error-bg: #FFF2F2;
    }

    * {
      box-sizing: border-box;
      -webkit-tap-highlight-color: transparent;
      -webkit-text-size-adjust: 100%;
    }

    html, body {
      width: 100%;
      overflow-x: hidden;
      scroll-behavior: smooth;
    }

    body {
      font-family: -apple-system, BlinkMacSystemFont, "SF Pro Text", "SF Pro Display", "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
      background-color: var(--bg);
      color: var(--text-main);
      margin: 0;
      padding: env(safe-area-inset-top, 12px) 14px calc(36px + env(safe-area-inset-bottom, 20px)) 14px;
      font-size: clamp(17px, 4.8vw, 20px);
      line-height: 1.45;
    }

    .container {
      width: 100%;
      max-width: 600px;
      margin: 0 auto;
    }

    .header-card {
      background: var(--card-bg);
      border-radius: 16px;
      padding: 16px 14px;
      margin-top: 6px;
      margin-bottom: 14px;
      text-align: center;
      box-shadow: 0 1px 4px rgba(0,0,0,0.06);
      border: 1px solid var(--border);
    }

    .hospital-title {
      font-size: clamp(0.95rem, 3.8vw, 1.1rem);
      color: #636366;
      font-weight: 600;
      margin-bottom: 4px;
    }

    .main-title {
      font-size: clamp(1.4rem, 5.5vw, 1.85rem);
      font-weight: 800;
      margin: 0 0 8px 0;
      color: var(--text-main);
      line-height: 1.25;
      white-space: pre-line;
    }

    .instruction {
      font-size: clamp(1.05rem, 4.2vw, 1.2rem);
      color: var(--text-muted);
      margin: 0;
      line-height: 1.5;
    }

    .question-card {
      background: var(--card-bg);
      border-radius: 16px;
      padding: 18px 14px;
      margin-bottom: 14px;
      box-shadow: 0 1px 4px rgba(0,0,0,0.05);
      border: 1.5px solid var(--border);
      transition: border-color 0.2s, background-color 0.2s;
    }

    .question-card.highlight-error {
      border-color: var(--error-border) !important;
      background-color: var(--error-bg);
      animation: pulse-border 1.5s infinite;
    }

    @keyframes pulse-border {
      0%, 100% { transform: scale(1); }
      50% { transform: scale(1.01); }
    }

    .question-title {
      font-size: clamp(1.2rem, 4.8vw, 1.4rem);
      font-weight: 700;
      color: var(--text-main);
      line-height: 1.35;
      margin-bottom: 12px;
    }

    .options-group {
      display: flex;
      flex-direction: column;
      gap: 10px;
    }

    .option-item {
      display: flex;
      align-items: center;
      padding: 14px 14px;
      border-radius: 14px;
      background: #F8F8FA;
      border: 2px solid transparent;
      cursor: pointer;
      font-size: clamp(1.05rem, 4.2vw, 1.2rem);
      color: #1C1C1E;
      transition: all 0.15s ease-in-out;
      user-select: none;
    }

    .option-item:active {
      transform: scale(0.99);
      background: #E5E5EA;
    }

    .option-item.selected {
      background: var(--selected-bg);
      border-color: var(--primary);
      color: #004CB8;
      font-weight: 700;
    }

    .custom-radio {
      width: 24px;
      height: 24px;
      border-radius: 50%;
      border: 2px solid #8E8E93;
      margin-right: 12px;
      flex-shrink: 0;
      display: flex;
      align-items: center;
      justify-content: center;
      background: #FFF;
      transition: border-color 0.15s, background-color 0.15s;
    }

    .option-item.selected .custom-radio {
      border-color: var(--primary);
      background: var(--primary);
    }

    .option-item.selected .custom-radio::after {
      content: "";
      width: 10px;
      height: 10px;
      background: #FFF;
      border-radius: 50%;
    }

    .option-text {
      flex-grow: 1;
      line-height: 1.4;
    }

    .hidden-radio {
      position: absolute;
      opacity: 0;
      pointer-events: none;
    }

    .calc-btn {
      margin-top: 16px;
      padding: 18px;
      width: 100%;
      background: var(--primary);
      color: #FFFFFF;
      font-size: clamp(1.2rem, 5vw, 1.35rem);
      font-weight: 700;
      border: none;
      border-radius: 16px;
      cursor: pointer;
      box-shadow: 0 4px 14px rgba(0, 122, 255, 0.35);
      transition: background 0.15s, transform 0.1s;
    }

    .calc-btn:active {
      background: var(--primary-active);
      transform: scale(0.98);
    }

    .modal {
      display: none;
      position: fixed;
      z-index: 999;
      inset: 0;
      background: rgba(0, 0, 0, 0.5);
      backdrop-filter: blur(8px);
      -webkit-backdrop-filter: blur(8px);
      align-items: center;
      justify-content: center;
      padding: 16px;
    }

    .modal-content {
      background: #FFFFFF;
      padding: 24px 18px;
      border-radius: 22px;
      width: 100%;
      max-width: 400px;
      text-align: center;
      box-shadow: 0 12px 36px rgba(0,0,0,0.25);
    }

    .modal-content h2 {
      margin: 0 0 12px 0;
      font-size: 1.6rem;
    }

    .result-block {
      background: #F8F8FA;
      border-radius: 14px;
      padding: 12px;
      margin-bottom: 12px;
    }

    .result-label {
      font-size: 1rem;
      color: var(--text-muted);
      margin: 0 0 4px 0;
    }

    .result-val {
      font-size: 2.2rem;
      font-weight: 800;
      color: var(--primary);
      margin: 0;
    }

    .risk-badge {
      display: inline-block;
      padding: 6px 16px;
      border-radius: 20px;
      font-size: 1.15rem;
      font-weight: 800;
      margin-top: 6px;
    }

    .risk-low { background: #E4F8E9; color: #28A745; }
    .risk-med { background: #FFF4E5; color: #E67E22; }
    .risk-high { background: #FDE8E8; color: #E74C3C; }

    .notice-box {
      background: #FFF9E6;
      border: 1.5px solid #FFD666;
      padding: 14px;
      margin: 16px 0;
      font-size: 1.05rem;
      color: #874D00;
      border-radius: 12px;
      line-height: 1.5;
      font-weight: 700;
    }

    .close-btn {
      width: 100%;
      padding: 16px;
      background: #1C1C1E;
      color: #FFF;
      font-size: 1.2rem;
      font-weight: 700;
      border: none;
      border-radius: 14px;
      cursor: pointer;
    }
  </style>
</head>
<body>

<div class="container">
  <div class="header-card">
    <div class="hospital-title" id="hospitalTitle"></div>
    <h1 class="main-title" id="formTitle"></h1>
    <p class="instruction" id="formInstruction"></p>
  </div>

  <form id="evaluationForm">
    <div id="questionsContainer"></div>
    
    <button type="button" class="calc-btn" onclick="submitAssessment()">Calculate Assessment Result</button>
  </form>
</div>

<div id="scoreModal" class="modal" role="dialog" aria-modal="true" aria-labelledby="modalTitle" onclick="handleBackdropClick(event)">
  <div class="modal-content" onclick="event.stopPropagation()">
    <h2 id="modalTitle">Assessment Result</h2>

    <div class="result-block">
      <p class="result-label" id="modalScoreTitle">STarT Back Screening Tool</p>
      <p id="modalScoreValue" class="result-val">0 / 9</p>
      <div id="riskLevelDisplay"></div>
      <p id="modalScoreSubtitle" style="font-size: 0.95rem; margin: 8px 0 0 0; color: var(--text-muted);"></p>
    </div>

    <div class="notice-box">
      Please do not leave this page and present it to your therapist.<br>
      Alternatively, you may take a screenshot.<br>
      Thank you.
    </div>
    
    <button type="button" class="close-btn" onclick="closeModal()">Close</button>
  </div>
</div>

<script>
const binaryOptions = [
  { key: "disagree", value: 0, label: "Disagree" },
  { key: "agree", value: 1, label: "Agree" }
];

const bothersomeOptions = [
  { key: "not_at_all", value: 0, label: "Not at all" },
  { key: "slightly", value: 0, label: "Slightly" },
  { key: "moderately", value: 0, label: "Moderately" },
  { key: "very_much", value: 1, label: "Very much" },
  { key: "extremely", value: 1, label: "Extremely" }
];

const CONFIG = {
  hospitalTitle: "Hospital Authority Pamela Youde Nethersole Eastern Hospital\nPhysiotherapy Department",
  title: "STarT Back Screening Tool",
  instruction: "Thinking about the <strong>last 2 weeks</strong>, select your response to the following questions:",
  items: [
    { title: "My back pain has spread down my leg(s) at some time in the last 2 weeks", options: binaryOptions },
    { title: "I have had pain in the shoulder or neck at some time in the last 2 weeks", options: binaryOptions },
    { title: "I have only walked short distances because of my back pain", options: binaryOptions },
    { title: "In the last 2 weeks, I have dressed more slowly than usual because of back pain", options: binaryOptions },
    { title: "It’s not really safe for a person with a condition like mine to be physically active", options: binaryOptions },
    { title: "Worrying thoughts have been going through my mind a lot of the time", options: binaryOptions },
    { title: "I feel that my back pain is terrible and it’s never going to get any better", options: binaryOptions },
    { title: "In general I have not enjoyed all the things I used to enjoy", options: binaryOptions },
    { title: "Overall, how bothersome has your back pain been in the last 2 weeks?", options: bothersomeOptions }
  ]
};

document.addEventListener("DOMContentLoaded", () => {
  initQuestionnaire();
});

function initQuestionnaire() {
  const hospEl = document.getElementById("hospitalTitle");
  if (CONFIG.hospitalTitle) {
    hospEl.textContent = CONFIG.hospitalTitle;
  } else {
    hospEl.style.display = "none";
  }

  document.getElementById("formTitle").textContent = CONFIG.title;
  document.getElementById("formInstruction").innerHTML = CONFIG.instruction;

  const container = document.getElementById("questionsContainer");
  container.innerHTML = "";

  CONFIG.items.forEach((item, index) => {
    const qNum = index + 1;
    const card = document.createElement("div");
    card.className = "question-card";
    card.id = `card-q${qNum}`;

    const title = document.createElement("div");
    title.className = "question-title";
    title.textContent = `${qNum}. ${item.title}`;
    card.appendChild(title);

    const optionsGroup = document.createElement("div");
    optionsGroup.className = "options-group";

    item.options.forEach(opt => {
      const label = document.createElement("label");
      label.className = "option-item";
      label.htmlFor = `q${qNum}_${opt.key}`;

      const radio = document.createElement("input");
      radio.type = "radio";
      radio.name = `q${qNum}`;
      radio.id = `q${qNum}_${opt.key}`;
      radio.value = opt.value;
      radio.className = "hidden-radio";

      radio.addEventListener("change", () => {
        optionsGroup.querySelectorAll(".option-item").forEach(el => el.classList.remove("selected"));
        if (radio.checked) {
          label.classList.add("selected");
          card.classList.remove("highlight-error");
        }
      });

      const dot = document.createElement("span");
      dot.className = "custom-radio";
      dot.setAttribute("aria-hidden", "true");

      const itemText = document.createElement("span");
      itemText.className = "option-text";
      itemText.textContent = opt.label;

      label.append(radio, dot, itemText);
      optionsGroup.appendChild(label);
    });

    card.appendChild(optionsGroup);
    container.appendChild(card);
  });
}

function submitAssessment() {
  const unanswered = [];
  document.querySelectorAll(".question-card").forEach(c => c.classList.remove("highlight-error"));

  for (let i = 1; i <= CONFIG.items.length; i++) {
    if (!document.querySelector(`input[name="q${i}"]:checked`)) {
      unanswered.push(i);
    }
  }

  if (unanswered.length > 0) {
    unanswered.forEach(num => {
      const card = document.getElementById(`card-q${num}`);
      if (card) card.classList.add("highlight-error");
    });

    const firstCard = document.getElementById(`card-q${unanswered[0]}`);
    if (firstCard) {
      firstCard.scrollIntoView({ behavior: "smooth", block: "center" });
    }
    return;
  }

  let totalScore = 0;
  let subScore = 0;

  for (let i = 1; i <= 9; i++) {
    const selected = document.querySelector(`input[name="q${i}"]:checked`);
    const val = parseInt(selected.value, 10);
    totalScore += val;
    if (i >= 5) {
      subScore += val;
    }
  }

  let riskCategory = "";
  let riskClass = "";

  if (totalScore <= 3) {
    riskCategory = "Low Risk (LR ≤ 3)";
    riskClass = "risk-low";
  } else {
    if (subScore >= 4) {
      riskCategory = "High Risk (HR: Sub-score ≥ 4)";
      riskClass = "risk-high";
    } else {
      riskCategory = "Medium Risk (MR)";
      riskClass = "risk-med";
    }
  }

  document.getElementById("modalScoreValue").textContent = `${totalScore} / 9`;
  document.getElementById("riskLevelDisplay").innerHTML = `<span class="risk-badge ${riskClass}">${riskCategory}</span>`;
  document.getElementById("modalScoreSubtitle").innerHTML = `Sub-score (Q5–Q9): <strong>${subScore}</strong> / 5`;

  const modal = document.getElementById("scoreModal");
  modal.style.display = "flex";
}

function closeModal() {
  document.getElementById("scoreModal").style.display = "none";
}

function handleBackdropClick(e) {
  if (e.target.id === "scoreModal") {
    closeModal();
  }
}

document.addEventListener("keydown", (e) => {
  if (e.key === "Escape") {
    closeModal();
  }
});
</script>

</body>
</html>
