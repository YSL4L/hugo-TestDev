---
title: "Teste dein Wissen"
description: "Bist du schon Experte*In in der Stadtforschung? Oder möchtest du einfach mal abfragen, was du bis jetzt weißt? Dann trau dich und versuche dich an meinem interaktiven Quiz zum Thema Stadtforschung!"
weight: 5
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
  const quizForm = document.getElementById("quiz-form");
  const questionElement = document.getElementById("question");
  const answersElement = document.getElementById("answers");
  const submitButton = document.getElementById("submit-btn");
  const nextButton = document.getElementById("next-btn");
  const feedbackElement = document.getElementById("feedback");

 // Quiz-Daten
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
  }
];

  let currentQuestion = 0;
  let score = 0;
  let selectedAnswer = null;

  function showQuestion() {
    const q = quizData[currentQuestion];
    questionElement.textContent = q.question;
    answersElement.innerHTML = "";
    
    q.options.forEach((opt, i) => {
      const btn = document.createElement("button");
      btn.type = "button";
      btn.className = "list-group-item list-group-item-action";
      btn.textContent = opt;
      btn.dataset.index = i;
      btn.addEventListener("click", () => {
        // Entferne 'active' von allen Buttons
        answersElement.querySelectorAll("button").forEach(b => b.classList.remove("active"));
        // Füge 'active' zum aktuellen Button hinzu
        btn.classList.add("active");
        // Speichere ausgewählten Index
        selectedAnswer = i;
        // Aktiviere Submit-Button
        submitButton.disabled = false;
      });
      
      answersElement.appendChild(btn);
    });
    
    // Zurücksetzen des Feedback-Bereichs
    feedbackElement.classList.add("visually-hidden");
    feedbackElement.innerHTML = "";
    
    // Buttons zurücksetzen
    submitButton.disabled = true;
    submitButton.classList.remove("d-none");
    nextButton.classList.add("d-none");
  }

  function checkAnswer(e) {
    e.preventDefault();
    
    if (selectedAnswer === null) return;
    
    const correctIdx = quizData[currentQuestion].correct;
    
    // Deaktiviere alle Answer-Buttons
    Array.from(answersElement.children).forEach((btn, i) => {
      btn.disabled = true;
      
      // Markiere die richtige Antwort grün
      if (i === correctIdx) {
        btn.classList.add("list-group-item-success");
      }
      
      // Markiere die falsche Auswahl rot (falls falsch gewählt)
      if (i === selectedAnswer && selectedAnswer !== correctIdx) {
        btn.classList.add("list-group-item-danger");
      }
    });
    
    // Zeige Feedback
    feedbackElement.classList.remove("visually-hidden");
    
    if (selectedAnswer === correctIdx) {
      feedbackElement.innerHTML = '<div class="alert alert-success">Richtig! Du hast die korrekte Antwort gewählt.</div>';
      score++; // Erhöhe den Score nur bei richtiger Antwort
    } else {
      feedbackElement.innerHTML = `
        <div class="alert alert-danger">
          Leider falsch. Die richtige Antwort ist: <strong>${quizData[currentQuestion].options[correctIdx]}</strong>
        </div>`;
    }
    
    // Deaktiviere Submit-Button, zeige Next-Button
    submitButton.disabled = true;
    nextButton.classList.remove("d-none");
  }

  function nextQuestion() {
    currentQuestion++;
    selectedAnswer = null; // Zurücksetzen für die nächste Frage
    
    if (currentQuestion < quizData.length) {
      showQuestion();
    } else {
      showResults();
    }
  }

  function showResults() {
    questionElement.textContent = "Quiz abgeschlossen!";
    answersElement.innerHTML = `
      <div class="alert alert-primary">
        <h4>Dein Ergebnis: <strong>${score} von ${quizData.length}</strong></h4>
        <p>${getScoreFeedback(score, quizData.length)}</p>
        <button type="button" class="btn btn-primary mt-3" id="restart-btn">Quiz neu starten</button>
      </div>`;
    
    // Restart-Button Event hinzufügen
    document.getElementById("restart-btn").addEventListener("click", restartQuiz);
    
    // Verstecke die Quiz-Buttons
    submitButton.classList.add("d-none");
    nextButton.classList.add("d-none");
    
    // Zeige Feedback-Bereich
    feedbackElement.classList.add("visually-hidden");
  }
  
  function getScoreFeedback(score, total) {
    const percentage = (score / total) * 100;
    
    if (percentage >= 90) return "Hervorragend! Du bist ein echter Stadtforschungs-Experte!";
    if (percentage >= 70) return "Sehr gut! Du hast ein solides Wissen über Stadtforschung.";
    if (percentage >= 50) return "Gut gemacht! Du hast die Grundlagen der Stadtforschung verstanden.";
    return "Kein Problem! Vielleicht möchtest du die Inhalte nochmal durchlesen und es erneut versuchen.";
  }
  
  function restartQuiz() {
    currentQuestion = 0;
    score = 0;
    selectedAnswer = null;
    showQuestion();
  }

  // Event-Listener für Form-Submit und Next-Button
  quizForm.addEventListener("submit", checkAnswer);
  nextButton.addEventListener("click", nextQuestion);

  // Quiz starten
  showQuestion();
});
</script>

{{< /rawhtml >}}
