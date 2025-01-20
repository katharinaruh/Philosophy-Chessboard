# Chessboard
the psychological-philosophical chessboard - a cognitive exchange

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
            grid-template-columns: repeat(8, 60px); /* 8 Spalten mit fester Breite */
            grid-template-rows: repeat(8, 60px);    /* 8 Reihen mit fester Höhe */
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
        /* Abwechselnde Farben für Schachbrettfelder */
        .chessboard .square:nth-child(odd):nth-child(odd),
        .chessboard .square:nth-child(even):nth-child(even) {
            background-color: #f0d9b5; /* Helles Feld */
        }
        .chessboard .square:nth-child(odd):nth-child(even),
        .chessboard .square:nth-child(even):nth-child(odd) {
            background-color: #8b4513; /* Dunkles Feld */
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
        }
        #entryContent img {
            max-width: 100%;
        }
        .image-container {
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
                <option value="♕">Weißer Dame (Philosoph)</option>
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
            
            <!-- Bild Hochladen -->
            <div class="image-container">
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
        const imageSizeSlider = document.getElementById("imageSizeSlider");
        const entries = JSON.parse(localStorage.getItem("entries")) || {}; // Lade Einträge aus dem LocalStorage
        let selectedSquare = null;

        // Erstelle Schachbrettfelder (zu Beginn leer)
        for
