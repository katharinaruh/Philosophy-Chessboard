# Philosophy-Chessboard
- a small philosophical game -

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Philosophical Chessboard</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
        }
        .chessboard {
            display: grid;
            grid-template-columns: repeat(8, 1fr);
            width: 480px;
            height: 480px;
            border: 2px solid #333;
        }
        .square {
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            font-size: 14px;
            color: white;
        }
        .square:nth-child(odd) {
            background-color: #8b4513;
        }
        .square:nth-child(even) {
            background-color: #f0d9b5;
        }
        .modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            display: none;
            justify-content: center;
            align-items: center;
        }
        .modal-content {
            background: white;
            padding: 20px;
            border-radius: 8px;
            text-align: center;
            max-width: 500px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        .close {
            margin-top: 10px;
            cursor: pointer;
            color: #333;
        }
    </style>
</head>
<body>
    <div class="chessboard" id="chessboard"></div>

    <div class="modal" id="modal">
        <div class="modal-content">
            <p id="question"></p>
            <button id="solutionButton">Show Solution</button>
            <p id="solution" style="display: none; margin-top: 10px; font-weight: bold;"></p>
            <div class="close" id="closeModal">Close</div>
        </div>
    </div>

    <script>
        const questions = [
            { question: "What is the meaning of life?", solution: "The answer depends on individual belief systems." },
            { question: "Is free will an illusion?", solution: "Some argue it's an illusion due to deterministic processes." },
            { question: "Can we ever truly know anything?", solution: "Epistemology explores this deeply, with varied conclusions." },
            { question: "What defines a good life?", solution: "Philosophers like Aristotle suggest virtue, while others focus on happiness." },
            { question: "Does morality depend on culture?", solution: "Cultural relativism suggests yes, but universalists disagree." },
            { question: "What is consciousness?", solution: "Consciousness remains one of philosophy's greatest mysteries." },
            { question: "Is there an objective reality?", solution: "Some argue reality is subjective, shaped by perception." },
            { question: "Are humans inherently good or evil?", solution: "Thinkers like Hobbes and Rousseau offer opposing views." },
            { question: "What is justice?", solution: "Justice varies from fairness to moral righteousness in philosophical debates." },
            { question: "Does life have inherent purpose?", solution: "Existentialists argue we must create our own purpose." }
        ];

        const chessboard = document.getElementById("chessboard");
        const modal = document.getElementById("modal");
        const questionElement = document.getElementById("question");
        const solutionElement = document.getElementById("solution");
        const solutionButton = document.getElementById("solutionButton");
        const closeModal = document.getElementById("closeModal");

        // Create chessboard squares
        for (let row = 0; row < 8; row++) {
            for (let col = 0; col < 8; col++) {
                const square = document.createElement("div");
                square.className = "square";
                if ((row + col) % 2 === 0) {
                    square.style.backgroundColor = "#f0d9b5";
                } else {
                    square.style.backgroundColor = "#8b4513";
                }
                square.addEventListener("click", () => {
                    const randomIndex = Math.floor(Math.random() * questions.length);
                    const selectedQuestion = questions[randomIndex];
                    questionElement.textContent = selectedQuestion.question;
                    solutionElement.textContent = selectedQuestion.solution;
                    solutionElement.style.display = "none";
                    modal.style.display = "flex";
                });
                chessboard.appendChild(square);
            }
        }

        // Show solution
        solutionButton.addEventListener("click", () => {
            solutionElement.style.display = "block";
        });

        // Close modal
        closeModal.addEventListener("click", () => {
            modal.style.display = "none";
        });
    </script>
</body>
</html>
