---
title: "Interaktives Quiz"
description: "Teste dein Wissen zur kulturanthropologischen Stadtforschung"
---

{{< rawhtml >}}

<section aria-labelledby="quiz-title" class="mb-5">
  <div class="container-lg">
    <h1
      id="quiz-title"
      class="display-5 fw-bold text-center slide-in-left mb-5"
    >
      Interaktives Quiz
    </h1>

    <!-- Soft-Card für Quiz-Inhalt -->
    <div class="soft-card p-4 p-md-5 shadow-sm rounded-4 bg-light">
      <form id="quiz-form" novalidate>
        <div class="mb-4">
          <h2 id="question" class="h5" tabindex="0">
            Wird per JS befüllt
          </h2>
          <div id="answers" class="list-group"></div>
        </div>

        <!-- Button-Leiste -->
        <div class="d-flex justify-content-between">
          <button
            type="submit"
            class="btn btn-primary"
            id="submit-btn"
          >
            Antwort prüfen
          </button>
          <button
            type="button"
            class="btn btn-outline-secondary d-none"
            id="next-btn"
          >
            Nächste Frage
          </button>
        </div>
      </form>

      <!-- Feedback-Bereich -->
      <div
        id="feedback"
        class="mt-4 visually-hidden"
        role="alert"
        aria-live="polite"
      ></div>
    </div>

  </div>
</section>

<script>
document.addEventListener("DOMContentLoaded", () => {
  const quizForm        = document.getElementById("quiz-form");
  const questionElement = document.getElementById("question");
  const answersElement  = document.getElementById("answers");
  const submitButton    = document.getElementById("submit-btn");
  const nextButton      = document.getElementById("next-btn");
  const feedbackElement = document.getElementById("feedback");

  const quizData = [
    {
      question: "Was bezeichnet der Begriff 'Forschungsfeld' in der Stadtforschung?",
      options: [
        "Einen geografisch abgegrenzten Raum zum Forschen",
        "Eine Formation aus der Beziehung von Forschungsgegenstand, Akteuren und Forschenden",
        "Ein Gebiet mit besonderem wissenschaftlichen Interesse",
        "Die Gesamtheit aller Forschungsmethoden",
      ],
      correct: 1,
    },
    {
      question: "Was ist ein 'Go-Along'?",
      options: [
        "Eine schriftliche Methode zur Erforschung von Stadtleben",
        "Eine Interviewmethode in festen Räumen",
        "Die Begleitung von Forschungspartnern bei ihren alltäglichen Wegen",
        "Eine statistische Erhebungsmethode",
      ],
      correct: 2,
    },
    {
      question: "Welche Bedeutung hat das 'Mapping' in der kulturanthropologischen Stadtforschung?",
      options: [
        "Die reine kartografische Erfassung von Straßen und Gebäuden",
        "Das Erstellen von digitalen Stadtplänen",
        "Die Visualisierung sozialer und räumlicher Praktiken",
        "Die Erfassung historischer Stadtgrenzen",
      ],
      correct: 2,
    },
    {
      question: "Was versteht Clifford Geertz unter einer 'dichten Beschreibung'?",
      options: [
        "Eine bloße Zusammenfassung empirischer Daten",
        "Eine detaillierte Interpretation von Handlungen im kulturellen Kontext",
        "Eine Liste von Beobachtungen ohne Interpretation",
        "Eine geografische Kartierung von Ereignissen",
      ],
      correct: 1,
    },
    {
      question: "Was sind 'urbane Assemblagen' im Kontext der Stadtforschung?",
      options: [
        "Städtische Bauprojekte zur Modernisierung",
        "Zusammenspiel von Menschen, Technologien und Räumen in der Stadt",
        "Festgelegte Stadtteile und ihre Verwaltungseinheiten",
        "Planungen von Stadtverwaltungen zur Verkehrsoptimierung",
      ],
      correct: 1,
    },
    {
      question: "Was bedeutet 'Teilhabe' in der Stadtforschung?",
      options: [
        "Die gesetzliche Registrierung von Bewohnern",
        "Das bloße Wohnen in einer Stadt",
        "Aktive Beteiligung an sozialen, kulturellen und politischen Prozessen in der Stadt",
        "Das Konsumieren von kulturellen Veranstaltungen",
      ],
      correct: 2,
    },
    {
      question: "Welche zentrale Rolle spielt WhatsApp in deinem Fallbeispiel zu internationalen Student*Innen in Rom?",
      options: [
        "Es ersetzt klassische Lehrmethoden an der Universität.",
        "Es ermöglicht internationale Vernetzung und fördert die Beteiligung an der Stadt.",
        "Es dient hauptsächlich der Verwaltung von Studienleistungen.",
        "Es wird verwendet, um formale Anträge bei der Stadt einzureichen.",
      ],
      correct: 1,
    },
  ];

  let currentQuestion = 0;
  let score = 0;

  function showQuestion() {
    const q = quizData[currentQuestion];
    questionElement.textContent = q.question;
    answersElement.innerHTML = "";
    q.options.forEach((opt,i) => {
      const btn = document.createElement("button");
      btn.type = "button";
      btn.className = "list-group-item list-group-item-action";
      btn.textContent = opt;
      btn.dataset.index = i;
      btn.addEventListener("click", selectAnswer);
      answersElement.append(btn);
    });
    feedbackElement.textContent = "";
    submitButton.disabled = true;
    nextButton.classList.add("d-none");
  }

  function selectAnswer(e) {
    answersElement.querySelectorAll("button").forEach(b=>b.classList.remove("active"));
    e.target.classList.add("active");
    submitButton.disabled = false;
  }

  function checkAnswer(e) {
    e.preventDefault();
    const selected = answersElement.querySelector(".active");
    const idx = +selected.dataset.index;
    const correctIdx = quizData[currentQuestion].correct;

    Array.from(answersElement.children).forEach((b,i) => {
      b.disabled = true;
      if (i===correctIdx) b.classList.add("list-group-item-success");
    });

    if (idx===correctIdx) {
      feedbackElement.innerHTML = '<div class="alert alert-success">Richtig!</div>';
      score++;
    } else {
      feedbackElement.innerHTML = `
        <div class="alert alert-danger">
          Leider falsch. Richtige Antwort: <strong>${quizData[currentQuestion].options[correctIdx]}</strong>
        </div>`;
      selected.classList.add("list-group-item-danger");
    }

    submitButton.disabled = true;
    nextButton.classList.remove("d-none");
  }

  function nextQuestion() {
    currentQuestion++;
    if (currentQuestion < quizData.length) showQuestion();
    else showResults();
  }

  function showResults() {
    questionElement.textContent = "Quiz abgeschlossen!";
    answersElement.innerHTML = `<div class="alert alert-primary">Dein Ergebnis: <strong>${score} von ${quizData.length}</strong></div>`;
    submitButton.classList.add("d-none");
    nextButton.classList.add("d-none");
  }

  quizForm.addEventListener("submit", checkAnswer);
  nextButton.addEventListener("click", nextQuestion);

  showQuestion();
});
</script>

{{< /rawhtml >}}
