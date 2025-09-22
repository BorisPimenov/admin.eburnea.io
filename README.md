<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Admin Panel - Live Poll</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            background-color: #121212;
            color: #e0e0e0;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            min-height: 100vh;
            text-align: center;
        }

        .admin-panel {
            background-color: #1e1e1e;
            padding: 40px;
            border-radius: 8px;
            border: 1px solid #333;
            box-shadow: 0 0 10px rgba(0,0,0,0.5);
            max-width: 400px;
            width: 90%;
        }

        .admin-button {
            padding: 15px 30px;
            font-size: 1.2rem;
            color: #fff;
            background-color: #cf6679;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
            font-weight: bold;
        }

        .admin-button:hover {
            background-color: #a63f53;
        }

        .status-message {
            margin-top: 20px;
            font-size: 0.9rem;
            color: #999;
        }
    </style>
</head>
<body>

<div class="admin-panel">
    <h1>Pannello Admin Votazione</h1>
    <p>Utilizza questo pannello per resettare la votazione in diretta per tutti gli utenti connessi.</p>
    <button class="admin-button" id="reset-button">Resetta Votazione</button>
    <div class="status-message" id="status-message">In attesa di connessione...</div>
</div>

<script>
    // Assicurati che questo URL corrisponda a quello nel file index.html
    const websocket = new WebSocket('ws://localhost:8080');
    const resetButton = document.getElementById('reset-button');
    const statusMessage = document.getElementById('status-message');

    resetButton.addEventListener('click', () => {
        if (websocket.readyState === WebSocket.OPEN) {
            // Invia il comando di reset al server
            websocket.send('RESET');
            statusMessage.textContent = 'Comando di reset inviato!';
            console.log('Comando di reset inviato al server.');
        } else {
            statusMessage.textContent = 'Errore: Connessione WebSocket non attiva.';
            console.error('Errore: Connessione WebSocket non attiva.');
        }
    });

    websocket.onopen = () => {
        statusMessage.textContent = 'Connesso al server TouchDesigner.';
        console.log('Connessione WebSocket stabilita.');
    };

    websocket.onmessage = (event) => {
        // Opzionale: gestire i messaggi di conferma dal server
        console.log('Messaggio ricevuto dal server:', event.data);
    };

    websocket.onclose = () => {
        statusMessage.textContent = 'Connessione chiusa. Prova a ricaricare la pagina.';
        console.log('Connessione WebSocket chiusa.');
    };

    websocket.onerror = (error) => {
        statusMessage.textContent = 'Errore di connessione. Controlla il server.';
        console.error('Errore WebSocket:', error);
    };
</script>

</body>
</html>
