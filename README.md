# jogo_da_forca 
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jogo da Forca</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f0f0f0;
            color: #333;
            margin: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
        }

        #game {
            text-align: center;
            max-width: 600px;
            width: 100%;
        }

        #word {
            font-size: 2em;
            margin: 20px;
        }

        #hint {
            margin: 10px;
            font-style: italic;
        }

        #guesses {
            margin: 10px;
            font-size: 1.2em;
        }

        #score {
            margin: 10px;
            font-size: 1.5em;
            font-weight: bold;
        }

        #message {
            margin: 10px;
            color: red;
        }

        input[type="text"] {
            font-size: 1em;
            padding: 5px;
            margin: 10px;
            width: 80px;
        }

        button {
            padding: 10px;
            font-size: 1em;
            cursor: pointer;
            margin: 5px;
        }
    </style>
</head>
<body>
    <div id="game">
        <h1>Jogo da Forca</h1>
        <div id="word">_ _ _ _</div>
        <div id="hint">Dica: </div>
        <div id="guesses">Letras já tentadas: </div>
        <div id="score">Pontos: 0</div>
        <div id="message"></div>
        <input type="text" id="guess" maxlength="1" placeholder="Digite uma letra">
        <button onclick="makeGuess()">Tentar</button>
        <button onclick="startNewGame()">Novo Jogo</button>
    </div>

    <script>
        const words = [
            { word: 'javascript', hint: 'Linguagem de programação usada para criar sites interativos.' },
            { word: 'html', hint: 'Linguagem de marcação para criar páginas web.' },
            { word: 'css', hint: 'Linguagem de estilo para definir a aparência de páginas web.' },
            { word: 'react', hint: 'Biblioteca JavaScript para construir interfaces de usuário.' }
        ];

        let selectedWord;
        let hiddenWord;
        let guessedLetters;
        let score;

        function startNewGame() {
            const randomIndex = Math.floor(Math.random() * words.length);
            selectedWord = words[randomIndex].word;
            hiddenWord = Array(selectedWord.length).fill('_');
            guessedLetters = [];
            score = 0;

            document.getElementById('hint').textContent = `Dica: ${words[randomIndex].hint}`;
            document.getElementById('word').textContent = hiddenWord.join(' ');
            document.getElementById('guesses').textContent = 'Letras já tentadas: ';
            document.getElementById('message').textContent = '';
            document.getElementById('score').textContent = `Pontos: ${score}`;
            document.getElementById('guess').value = '';
            document.getElementById('guess').focus();
        }

        function makeGuess() {
            const guessInput = document.getElementById('guess');
            const guess = guessInput.value.toLowerCase();

            if (guess.length !== 1 || !/^[a-z]$/.test(guess)) {
                document.getElementById('message').textContent = 'Digite uma letra válida.';
                return;
            }

            if (guessedLetters.includes(guess)) {
                document.getElementById('message').textContent = 'Você já tentou esta letra.';
                return;
            }

            guessedLetters.push(guess);

            if (selectedWord.includes(guess)) {
                for (let i = 0; i < selectedWord.length; i++) {
                    if (selectedWord[i] === guess) {
                        hiddenWord[i] = guess;
                    }
                }
                document.getElementById('message').textContent = `Boa! A letra '${guess}' está correta.`;
            } else {
                document.getElementById('message').textContent = `A letra '${guess}' não está na palavra.`;
            }

            document.getElementById('word').textContent = hiddenWord.join(' ');
            document.getElementById('guesses').textContent = `Letras já tentadas: ${guessedLetters.join(', ')}`;

            if (!hiddenWord.includes('_')) {
                score += 10; // Incrementa a pontuação ao ganhar
                document.getElementById('score').textContent = `Pontos: ${score}`;
                document.getElementById('message').textContent = 'Parabéns, você ganhou!';
                setTimeout(startNewGame, 2000); // Começa um novo jogo após 2 segundos
            }

            guessInput.value = '';
            guessInput.focus();
        }

        // Adiciona um ouvinte de eventos para a tecla Enter
        document.getElementById('guess').addEventListener('keydown', (event) => {
            if (event.key === 'Enter') {
                makeGuess();
                event.preventDefault(); // Evita o envio do formulário
            }
        });

        // Inicia o primeiro jogo
        startNewGame();
    </script>
</body>
</html>
