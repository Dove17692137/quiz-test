<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz ze słówek</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; padding: 20px; background-color: #f4f4f4; margin: 0; }
        .container { max-width: 90%; margin: auto; background: white; padding: 20px; border-radius: 10px; box-shadow: 0 0 10px rgba(0, 0, 0, 0.1); }
        .question { font-size: 1.2em; margin-bottom: 10px; }
        input { width: calc(100% - 20px); padding: 8px; margin: 10px 0; font-size: 1em; }
        button { padding: 10px 20px; font-size: 1em; cursor: pointer; border: none; border-radius: 5px; background: #007BFF; color: white; }
        button:hover { background: #0056b3; }
        #summary { display: none; }
        #feedback { font-size: 1.2em; font-weight: bold; margin-top: 10px; }
    </style>
</head>
<body>
    <div class="container">
        <h1>Quiz ze słówek</h1>
        <div id="quiz-container">
            <div class="question" id="question">Pytanie</div>
            <input type="text" id="answer" placeholder="Wpisz odpowiedź" onkeypress="handleKeyPress(event)">
            <button id="check-button" onclick="checkAnswer()">Sprawdź</button>
            <p id="feedback"></p>
        </div>
        <div id="summary">
            <h2>Podsumowanie</h2>
            <p id="score"></p>
            <ul id="answers-list"></ul>
        </div>
    </div>
    
    <script>
        const words = [
            { en: "alpine pass", pl: "przełęcz wysokogórska" },
            { en: "coast", pl: "brzeg" },
            { en: "deep", pl: "głębokość" },
            { en: "doubtful", pl: "wątpliwy" },
            { en: "fan", pl: "fan" },
            { en: "fine", pl: "świetny" },
            { en: "glacier", pl: "lodowiec" },
            { en: "landscape", pl: "krajobraz" },
            { en: "narrow", pl: "wąski" },
            { en: "recognise", pl: "rozpoznać" },
            { en: "setting", pl: "sceneria" },
            { en: "steep", pl: "stromy" },
            { en: "tour", pl: "wycieczka" },
            { en: "travel destination", pl: "cel podróży" },
            { en: "wetlands", pl: "podmokłe tereny" },
            { en: "mountain range", pl: "pasmo górskie" },
            { en: "thin", pl: "cienki, chudy" },
            { en: "improve", pl: "poprawiać, ulepszać" },
            { en: "realise", pl: "zdanie sobie sprawy" },
            { en: "to trek", pl: "wędrować, podróżować" },
            { en: "lecture", pl: "wykład" },
            { en: "facilities", pl: "obiekty użyteczności, udogodnienia" },
            { en: "statement", pl: "twierdzenie, oświadczenie" },
            { en: "porters", pl: "bagażowy" },
            { en: "sail", pl: "pływać statkiem" }
        ];
        
        let totalQuestions = words.length;
        let availableWords = [...words];
        let answeredQuestions = [];
        let correctAnswers = 0;
        let currentQuestion;
        
        function getRandomQuestion() {
            if (availableWords.length === 0) return null;
            const index = Math.floor(Math.random() * availableWords.length);
            return availableWords.splice(index, 1)[0];
        }
        
        function loadQuestion() {
            if (answeredQuestions.length >= totalQuestions || availableWords.length === 0) {
                showSummary();
                return;
            }
            
            currentQuestion = getRandomQuestion();
            if (!currentQuestion) return;
            
            document.getElementById("question").textContent = `Podaj angielskie tłumaczenie: ${currentQuestion.pl}`;
            document.getElementById("feedback").textContent = "";
            document.getElementById("answer").value = "";
        }
        
        function checkAnswer() {
            const userAnswer = document.getElementById("answer").value.trim().toLowerCase();
            const feedback = document.getElementById("feedback");
            const correctAnswer = currentQuestion.en.toLowerCase();
            
            if (userAnswer === correctAnswer) {
                feedback.textContent = "Dobrze!";
                feedback.style.color = "green";
                correctAnswers++;
            } else {
                feedback.textContent = `Błąd! Poprawna odpowiedź to: ${currentQuestion.en}`;
                feedback.style.color = "red";
            }
            
            answeredQuestions.push({
                question: currentQuestion.pl,
                chosen: userAnswer || "[brak]",
                correct: currentQuestion.en
            });
            
            setTimeout(loadQuestion, 1000);
        }
        
        function showSummary() {
            document.getElementById("quiz-container").style.display = "none";
            document.getElementById("summary").style.display = "block";
            
            document.getElementById("score").textContent = `Twój wynik: ${correctAnswers}/${totalQuestions}`;
            
            const list = document.getElementById("answers-list");
            list.innerHTML = "";
            answeredQuestions.forEach(item => {
                const li = document.createElement("li");
                li.textContent = `${item.question} - Twoja odpowiedź: ${item.chosen} (Poprawna: ${item.correct})`;
                list.appendChild(li);
            });
        }
        
        function handleKeyPress(event) {
            if (event.key === "Enter") {
                checkAnswer();
            }
        }
        
        loadQuestion();
    </script>
</body>
</html>


