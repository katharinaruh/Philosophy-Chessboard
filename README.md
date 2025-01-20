# Chessboard
the psychological-philosophical chessboard - a cognitive exchange

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Philosophers and Psychologists Chessboard</title>
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
            grid-template-columns: repeat(8, 1fr);
            width: 480px;
            height: 480px;
            border: 2px solid #333;
            margin-bottom: 20px;
        }
        .square {
            width: 60px;
            height: 60px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 24px;
            cursor: pointer;
        }
        .square:nth-child(even) {
            background-color: #f0d9b5;
        }
        .square:nth-child(odd) {
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
    </style>
</head>
<body>
    <h1>Philosophers and Psychologists Chessboard</h1>
    <div class="chessboard" id="chessboard"></div>

    <div class="modal" id="modal">
        <div class="modal-content">
            <h2 id="modalTitle">Place a Piece</h2>
            <label for="pieceType">Choose a Piece:</label>
            <select id="pieceType">
                <option value="♔">White King (Philosopher)</option>
                <option value="♕">White Queen (Philosopher)</option>
                <option value="♖">White Rook (Philosopher)</option>
                <option value="♗">White Bishop (Philosopher)</option>
                <option value="♘">White Knight (Philosopher)</option>
                <option value="♙">White Pawn (Philosopher)</option>
                <option value="♚">Black King (Psychologist)</option>
                <option value="♛">Black Queen (Psychologist)</option>
                <option value="♜">Black Rook (Psychologist)</option>
                <option value="♝">Black Bishop (Psychologist)</option>
                <option value="♞">Black Knight (Psychologist)</option>
                <option value="♟">Black Pawn (Psychologist)</option>
            </select>
            <label for="title">Title:</label>
            <input type="text" id="entryTitle" placeholder="Enter a title...">
            <label for="content">Text:</label>
            <textarea id="entryContent" rows="4" placeholder="Enter your text..."></textarea>
            <button id="saveEntry">Save</button>
            <button id="cancelEntry">Cancel</button>
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
        const entries = JSON.parse(localStorage.getItem("entries")) || {}; // Load entries from localStorage
        let selectedSquare = null;

        // Create chessboard squares (initially empty)
        for (let row = 0; row < 8; row++) {
            for (let col = 0; col < 8; col++) {
                const square = document.createElement("div");
                square.className = "square";
                square.dataset.position = `${row}-${col}`;

                // Add event listener to open the modal when clicked
                square.addEventListener("click", () => openModal(square));
                chessboard.appendChild(square);
            }
        }

        // Open modal to create/edit an entry
        function openModal(square) {
            selectedSquare = square;
            const position = square.dataset.position;
            if (entries[position]) {
                entryTitle.value = entries[position].title;
                entryContent.value = entries[position].content;
                pieceType.value = entries[position].piece;
            } else {
                entryTitle.value = "";
                entryContent.value = "";
                pieceType.value = "♔"; // Default to white king
            }
            modal.style.display = "flex";
        }

        // Save entry
        saveEntry.addEventListener("click", () => {
            const title = entryTitle.value.trim();
            const content = entryContent.value.trim();
            const piece = pieceType.value;
            if (!title || !content) {
                alert("Please fill in both title and text.");
                return;
            }

            const position = selectedSquare.dataset.position;
            entries[position] = {
                title,
                content,
                piece,
                timestamp: new Date().toLocaleString()
            };

            // Save to localStorage
            localStorage.setItem("entries", JSON.stringify(entries));

            // Update the square with the selected piece
            selectedSquare.textContent = entries[position].piece;
            modal.style.display = "none";
        });

        // Cancel entry
        cancelEntry.addEventListener("click", () => {
            modal.style.display = "none";
        });

        // Show entry on square click
        chessboard.addEventListener("click", (e) => {
            const square = e.target;
            const position = square.dataset.position;
            if (entries[position]) {
                const entry = entries[position];
                alert(`Title: <span class="entry-title">${entry.title}</span>\nText: ${entry.content}\nPiece: ${entry.piece}\nTimestamp: ${entry.timestamp}`);
            }
        });
    </script>
</body>
</html>
