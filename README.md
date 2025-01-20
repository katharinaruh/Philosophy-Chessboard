<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Schachbrett der Philosophen und Psychologen</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            align-items: center;
            font-family: Times New Roman, serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 20px;
        }
        .chessboard {
            display: grid;
            grid-template-columns: repeat(8, 60px);
            grid-template-rows: repeat(8, 60px);
            width: 480px;
            height: 480px;
            border: 2px solid #333;
            margin-bottom: 20px;
        }
        .square {
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 24px;
            cursor: pointer;
        }
        .chessboard .square:nth-child(odd):nth-child(odd),
        .chessboard .square:nth-child(even):nth-child(even) {
            background-color: #f0d9b5;
        }
        .chessboard .square:nth-child(odd):nth-child(even),
        .chessboard .square:nth-child(even):nth-child(odd) {
            background-color: #8b4513;
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
            max-width: 500px;
            width: 100%;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        .modal-content input, .modal-content textarea, .modal-content select {
            width: 100%;
            margin-bottom: 10px;
            padding: 8px;
            font-family: Times New Roman, serif;
        }
        .modal-content button {
            padding: 8px 12px;
            font-size: 14px;
            cursor: pointer;
        }
        .entry-title {
            font-weight: bold;
        }
        #entryContent {
            width: 100%;
            min-height: 150px;
            border: 1px solid #ccc;
            padding: 8px;
            font-family: Times New Roman, serif;
            overflow-y: auto;
            max-height: 300px;
        }
        #entryContent img {
            max-width: 100%;
        }
        .file-container {
            margin-bottom: 10px;
        }
        #imageSizeSlider {
            width: 100%;
        }
    </style>
</head>
<body>
    <h1>Schachbrett der Philosophen und Psychologen</h1>
    <div class="chessboard" id="chessboard"></div>

    <div class="modal" id="modal">
        <div class="modal-content">
            <h2 id="modalTitle">Setze eine Figur</h2>
            <label for="pieceType">Wähle eine Figur:</label>
            <select id="pieceType">
                <option value="♔">Weißer König (Philosoph)</option>
                <option value="♕">Weiße Dame (Philosoph)</option>
                <option value="♖">Weißer Turm (Philosoph)</option>
                <option value="♗">Weißer Läufer (Philosoph)</option>
                <option value="♘">Weißes Springer (Philosoph)</option>
                <option value="♙">Weißer Bauer (Philosoph)</option>
                <option value="♚">Schwarzer König (Psychologe)</option>
                <option value="♛">Schwarze Dame (Psychologe)</option>
                <option value="♜">Schwarzer Turm (Psychologe)</option>
                <option value="♝">Schwarzer Läufer (Psychologe)</option>
                <option value="♞">Schwarzes Springer (Psychologe)</option>
                <option value="♟">Schwarzer Bauer (Psychologe)</option>
            </select>
            <label for="title">Titel:</label>
            <input type="text" id="entryTitle" placeholder="Gib einen Titel ein...">
            <label for="content">Text:</label>
            <div id="entryContent" contenteditable="true" placeholder="Gib deinen Text ein..."></div>

            <!-- Datei Hochladen -->
            <div class="file-container">
                <label for="fileUpload">Datei hochladen:</label>
                <input type="file" id="fileUpload" accept="image/*,.pdf,.doc,.docx,.txt">
            </div>

            <!-- Bild Hochladen -->
            <div class="file-container">
                <label for="imageUpload">Bild hochladen:</label>
                <input type="file" id="imageUpload" accept="image/*">
                <br>
                <label for="imageSizeSlider">Bildgröße:</label>
                <input type="range" id="imageSizeSlider" min="10" max="500" value="100">
            </div>

            <button id="saveEntry">Speichern</button>
            <button id="cancelEntry">Abbrechen</button>
        </div>
    </div>

    <script>
        const chessboard = document.getElementById("chessboard");
        const modal = document.getElementById("modal");
        const entryTitle = document.getElementById("entryTitle");
        const entryContent = document.getElementById("entryContent");
        const pieceType = document.getElementById("pieceType");
        const saveEntry = document.getElementById("saveEntry");
        const cancelEntry = document.getElementById("cancelEntry");
        const imageUpload = document.getElementById("imageUpload");
        const fileUpload = document.getElementById("fileUpload");
        const imageSizeSlider = document.getElementById("imageSizeSlider");
        const entries = JSON.parse(localStorage.getItem("entries")) || {}; 
        let selectedSquare = null;

        for (let row = 0; row < 8; row++) {
            for (let col = 0; col < 8; col++) {
                const square = document.createElement("div");
                square.className = "square";
                square.dataset.position = `${row}-${col}`;
                if ((row + col) % 2 === 0) {
                    square.style.backgroundColor = "#f0d9b5";
                } else {
                    square.style.backgroundColor = "#8b4513";
                }
                if (entries[`${row}-${col}`]) {
                    square.textContent = entries[`${row}-${col}`].piece;
                }
                square.addEventListener("click", () => openModal(square));
                chessboard.appendChild(square);
            }
        }

        function openModal(square) {
            selectedSquare = square;
            const position = square.dataset.position;
            if (entries[position]) {
                entryTitle.value = entries[position].title;
                entryContent.innerHTML = entries[position].content;
                pieceType.value = entries[position].piece;
            } else {
                entryTitle.value = "";
                entryContent.innerHTML = "";
                pieceType.value = "♔";
            }
            modal.style.display = "flex";
        }

        imageUpload.addEventListener("change", () => {
            const file = imageUpload.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(event) {
                    const img = document.createElement("img");
                    img.src = event.target.result;
                    img.style.maxWidth = `${imageSizeSlider.value}%`;
                    img.style.height = "auto";
                    entryContent.appendChild(img);
                };
                reader.readAsDataURL(file);
            }
        });

        fileUpload.addEventListener("change", () => {
            const file = fileUpload.files[0];
            if (file) {
                const fileName = document.createElement("p");
                fileName.textContent = `Datei: ${file.name}`;
                entryContent.appendChild(fileName);
            }
        });

        imageSizeSlider.addEventListener("input", () => {
            const img = entryContent.querySelector("img");
            if (img) {
                img.style.maxWidth = `${imageSizeSlider.value}%`;
            }
        });

        saveEntry.addEventListener("click", () => {
            const title = entryTitle.value.trim();
            const content = entryContent.innerHTML.trim();
            const piece = pieceType.value;
            if (!title || !content) {
                alert("Bitte sowohl Titel als auch Text ausfüllen.");
                return;
            }

            const position = selectedSquare.dataset.position;
            entries[position] = {
                title,
                content,
                piece,
                timestamp: new Date().toLocaleString()
            };

            localStorage.setItem("entries", JSON.stringify(entries));
            selectedSquare.textContent = entries[position].piece;
            modal.style.display = "none";
        });

        cancelEntry.addEventListener("click", () => {
            modal.style.display = "none";
        });
    </script>
</body>
</html>
