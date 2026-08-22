---
layout: default
title: Verb Conjugation Drill
---

<style>
  .drill-container {
    width: 100%;
    max-width: 650px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    gap: 16px;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
  }
  .drill-card {
    background-color: #1e293b;
    border: 1px solid #334155;
    border-radius: 12px;
    padding: 24px;
    color: #f8fafc;
    box-shadow: 0 4px 6px -1px rgba(0,0,0,0.3);
  }
  .drill-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: #94a3b8;
    font-size: 0.9rem;
  }
  .drill-streak {
    color: #38bdf8;
    font-weight: bold;
  }
  .drill-prompt-box {
    text-align: center;
    margin: 24px 0;
  }
  .drill-infinitive {
    font-size: 2.2rem;
    font-weight: 700;
    color: #38bdf8;
    margin-bottom: 4px;
  }
  .drill-meaning {
    font-size: 1.1rem;
    color: #94a3b8;
    margin-bottom: 16px;
  }
  .drill-tags {
    display: flex;
    justify-content: center;
    gap: 8px;
    flex-wrap: wrap;
  }
  .drill-tag {
    background: #334155;
    color: #f8fafc;
    padding: 4px 10px;
    border-radius: 6px;
    font-size: 0.85rem;
    font-weight: 600;
  }
  .drill-tag.polarity {
    background: #475569;
  }
  .drill-input-row {
    display: flex;
    gap: 8px;
  }
  #drill-user-input {
    flex: 1;
    padding: 12px 16px;
    border-radius: 8px;
    border: 1px solid #334155;
    background: #0b1120;
    color: #f8fafc;
    font-size: 1.1rem;
    outline: none;
  }
  #drill-user-input:focus {
    border-color: #38bdf8;
  }
  #drill-submit-btn {
    background: #38bdf8;
    color: #0f172a;
    border: none;
    padding: 12px 20px;
    border-radius: 8px;
    font-weight: 600;
    font-size: 1rem;
    cursor: pointer;
  }
  #drill-submit-btn:hover {
    opacity: 0.9;
  }
  .drill-feedback {
    margin-top: 16px;
    padding: 12px;
    border-radius: 8px;
    text-align: center;
    font-size: 1rem;
    display: none;
  }
  .drill-feedback.correct {
    display: block;
    background: rgba(34, 197, 94, 0.15);
    color: #22c55e;
    border: 1px solid #22c55e;
  }
  .drill-feedback.wrong {
    display: block;
    background: rgba(239, 68, 68, 0.15);
    color: #ef4444;
    border: 1px solid #ef4444;
  }
  .drill-reference {
    font-size: 0.85rem;
    color: #94a3b8;
    line-height: 1.5;
  }
  .drill-reference details summary {
    cursor: pointer;
    color: #38bdf8;
    margin-bottom: 8px;
  }
</style>

<div class="drill-container">
  <div class="drill-card">
    <div class="drill-header">
      <span>Conjugation Drill (Lessons 1–8)</span>
      <span class="drill-streak" id="drill-score">Score: 0 | Streak: 0</span>
    </div>

    <div class="drill-prompt-box">
      <div class="drill-infinitive" id="drill-verb">...</div>
      <div class="drill-meaning" id="drill-translation">...</div>
      <div class="drill-tags">
        <div class="drill-tag" id="drill-tense-tag">Tense</div>
        <div class="drill-tag" id="drill-person-tag">Person</div>
        <div class="drill-tag polarity" id="drill-polarity-tag">Affirmative</div>
      </div>
    </div>

    <form id="drill-form" onsubmit="handleDrillCheck(event)">
      <div class="drill-input-row">
        <input type="text" id="drill-user-input" placeholder="Type Eastern Armenian..." autocomplete="off" autofocus>
        <button type="submit" id="drill-submit-btn">Check</button>
      </div>
    </form>

    <div id="drill-feedback-box" class="drill-feedback"></div>
  </div>

  <div class="drill-card drill-reference">
    <details>
      <summary>Grammar Rule Quick Reference</summary>
      <ul>
        <li><strong>Auxiliary Verb (Present):</strong> եմ, ես, է, ենք, եք, են | Negative: չեմ, չես, չի, չենք, չեք, չեն</li>
        <li><strong>Present Tense:</strong> Stem + <strong>ում</strong> + Aux (e.g., կարդում եմ / չեմ կարդում)</li>
        <li><strong>Past Participle (-ել):</strong> Stem + <strong>ել</strong> + Aux (e.g., ապրել եմ / չեմ ապրել)</li>
        <li><strong>Past Participle (-ալ):</strong> Stem + <strong>ացել</strong> + Aux (e.g., կարդացել եմ / չեմ կարդացել)</li>
        <li><strong>Future Tense:</strong> Infinitive + <strong>ու</strong> + Aux (e.g., գրելու եմ / չեմ գրելու)</li>
      </ul>
    </details>
  </div>
</div>

<script>
const drillVerbs = [
  { inf: "ապրել", eng: "to live", type: "el" },
  { inf: "ծանոթանալ", eng: "to meet / get acquainted", type: "al" },
  { inf: "ուտել", eng: "to eat", type: "el" },
  { inf: "իջնել", eng: "to go down", type: "el" },
  { inf: "կարդալ", eng: "to read", type: "al" },
  { inf: "ներկայանալ", eng: "to introduce oneself", type: "al" },
  { inf: "ցանկանալ", eng: "to wish", type: "al" },
  { inf: "գրել", eng: "to write", type: "el" },
  { inf: "խմել", eng: "to drink", type: "el" },
  { inf: "երգել", eng: "to sing", type: "el" },
  { inf: "պարել", eng: "to dance", type: "el" },
  { inf: "վազել", eng: "to run", type: "el" },
  { inf: "մտածել", eng: "to think", type: "el" },
  { inf: "զգալ", eng: "to feel", type: "al" },
  { inf: "գնալ", eng: "to go", type: "al" },
  { inf: "լսել", eng: "to listen", type: "el" },
  { inf: "աշխատել", eng: "to work", type: "el" },
  { inf: "դասավանդել", eng: "to teach", type: "el" },
  { inf: "խաղալ", eng: "to play", type: "al" },
  { inf: "սովորել", eng: "to study / learn", type: "el" },
  { inf: "օգնել", eng: "to help", type: "el" },
  { inf: "գնել", eng: "to buy", type: "el" },
  { inf: "մտնել", eng: "to enter", type: "el" },
  { inf: "լվանալ", eng: "to wash", type: "al" },
  { inf: "գտնել", eng: "to find", type: "el" },
  { inf: "եփել", eng: "to cook", type: "el" },
  { inf: "զբոսնել", eng: "to take a walk", type: "el" },
  { inf: "ծանոթացնել", eng: "to introduce", type: "el" },
  { inf: "ժամանել", eng: "to arrive", type: "el" },
  { inf: "խոսել", eng: "to speak", type: "el" },
  { inf: "քայլել", eng: "to walk", type: "el" },
  { inf: "մոտենալ", eng: "to approach", type: "al" },
  { inf: "զարմանալ", eng: "to be surprised", type: "al" },
  { inf: "թռչել", eng: "to fly", type: "el" },
  { inf: "ճաշել", eng: "to dine", type: "el" },
  { inf: "ցավել", eng: "to ache", type: "el" },
  { inf: "ամրագրել", eng: "to book", type: "el" },
  { inf: "այցելել", eng: "to visit", type: "el" },
  { inf: "անցկացնել", eng: "to pass / spend", type: "el" },
  { inf: "հանգստանալ", eng: "to rest", type: "al" },
  { inf: "հաշվառել", eng: "to register / check", type: "el" },
  { inf: "ձևակերպել", eng: "to formulate / process", type: "el" },
  { inf: "ճամփորդել", eng: "to travel", type: "el" },
  { inf: "մեկնել", eng: "to depart", type: "el" },
  { inf: "մնալ", eng: "to stay", type: "al" },
  { inf: "նախաճաշել", eng: "to have breakfast", type: "el" },
  { inf: "վերցնել", eng: "to take", type: "el" },
  { inf: "փակել", eng: "to close", type: "el" }
];

const drillPersons = [
  { label: "Ես (1st Sing.)", pos: "եմ", neg: "չեմ" },
  { label: "Դու (2nd Sing.)", pos: "ես", neg: "չես" },
  { label: "Նա (3rd Sing.)", pos: "է", neg: "չի" },
  { label: "Մենք (1st Plur.)", pos: "ենք", neg: "չենք" },
  { label: "Դուք (2nd Plur.)", pos: "եք", neg: "չեք" },
  { label: "Նրանք (3rd Plur.)", pos: "են", neg: "չեն" }
];

const drillTenses = ["Present", "Past", "Future"];
const drillPolarities = ["Affirmative", "Negative"];

let drillCurrent = null;
let drillScoreVal = 0;
let drillStreakVal = 0;
let drillAnswered = false;

function generateDrillTarget(verbObj, personObj, tense, polarity) {
  let participle = "";
  const stem = verbObj.inf.slice(0, -2);

  if (tense === "Present") {
    participle = stem + "ում";
  } else if (tense === "Past") {
    participle = verbObj.type === "al" ? stem + "ացել" : stem + "ել";
  } else if (tense === "Future") {
    participle = verbObj.inf + "ու";
  }

  if (polarity === "Affirmative") {
    return `${participle} ${personObj.pos}`;
  } else {
    return `${personObj.neg} ${participle}`;
  }
}

function loadNextDrill() {
  drillAnswered = false;
  const verb = drillVerbs[Math.floor(Math.random() * drillVerbs.length)];
  const person = drillPersons[Math.floor(Math.random() * drillPersons.length)];
  const tense = drillTenses[Math.floor(Math.random() * drillTenses.length)];
  const polarity = drillPolarities[Math.floor(Math.random() * drillPolarities.length)];

  drillCurrent = {
    verb,
    person,
    tense,
    polarity,
    answer: generateDrillTarget(verb, person, tense, polarity)
  };

  document.getElementById("drill-verb").textContent = verb.inf;
  document.getElementById("drill-translation").textContent = verb.eng;
  document.getElementById("drill-tense-tag").textContent = tense;
  document.getElementById("drill-person-tag").textContent = person.label;
  document.getElementById("drill-polarity-tag").textContent = polarity;

  const input = document.getElementById("drill-user-input");
  input.value = "";
  input.disabled = false;
  input.focus();

  const feedback = document.getElementById("drill-feedback-box");
  feedback.className = "drill-feedback";
  feedback.textContent = "";

  document.getElementById("drill-submit-btn").textContent = "Check";
}

function handleDrillCheck(e) {
  e.preventDefault();
  if (drillAnswered) {
    loadNextDrill();
    return;
  }

  const input = document.getElementById("drill-user-input");
  const userVal = input.value.trim().replace(/\s+/g, ' ');
  const feedback = document.getElementById("drill-feedback-box");

  if (userVal === drillCurrent.answer) {
    drillScoreVal += 10;
    drillStreakVal += 1;
    feedback.className = "drill-feedback correct";
    feedback.textContent = "Ճիշտ է (Correct)! Well done.";
  } else {
    drillStreakVal = 0;
    feedback.className = "drill-feedback wrong";
    feedback.textContent = `Սխալ է: Correct answer: ${drillCurrent.answer}`;
  }

  document.getElementById("drill-score").textContent = `Score: ${drillScoreVal} | Streak: ${drillStreakVal}`;
  document.getElementById("drill-submit-btn").textContent = "Next (Enter)";
  drillAnswered = true;
}

document.addEventListener("DOMContentLoaded", loadNextDrill);
if (document.readyState === "complete" || document.readyState === "interactive") {
  loadNextDrill();
}
</script>
