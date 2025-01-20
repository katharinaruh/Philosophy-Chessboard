# Philosophy-Chessboard

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
            grid-template-columns: repeat(8, 60px);
            grid-template-rows: repeat(8, 60px);
            border: 2px solid #333;
        }
        .square {
            width: 60px;
            height: 60px;
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
            // Add more questions and solutions up to 300
        ];

        const chessboard = document.getElementById("chessboard");
        const modal = document.getElementById("modal");
        const questionElement = document.getElementById("question");
        const solutionElement = document.getElementById("solution");
        const solutionButton = document.getElementById("solutionButton");
        const closeModal = document.getElementById("closeModal");

        // Create chessboard squares
        for (let i = 0; i < 64; i++) {
            const square = document.createElement("div");
            square.className = "square";
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
