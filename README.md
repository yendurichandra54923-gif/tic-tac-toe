<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Blue Tic Tac Toe</title>
  <style>
    body {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: linear-gradient(135deg, #1e3c72, #2a5298);
      color: #fff;
      transition: background 0.5s;
    }

    h1 {
      margin-bottom: 25px;
      font-size: 2.5rem;
      text-shadow: 2px 2px 8px rgba(0,0,0,0.5);
    }

    .board {
      display: grid;
      grid-template-columns: repeat(3, 120px);
      grid-template-rows: repeat(3, 120px);
      gap: 10px;
      margin-bottom: 20px;
    }

    .cell {
      width: 120px;
      height: 120px;
      background-color: rgba(255, 255, 255, 0.1);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 3rem;
      cursor: pointer;
      border-radius: 15px;
      box-shadow: 0 5px 15px rgba(0,0,0,0.3);
      transition: transform 0.2s, background-color 0.3s, color 0.3s;
    }

    .cell:hover {
      transform: scale(1.1);
      background-color: rgba(255, 255, 255, 0.2);
    }

    .cell.X {
      color: #00ffff; /* Bright Cyan */
      animation: fadeIn 0.3s ease-in;
    }

    .cell.O {
      color: #ff4da6; /* Neon Pink */
      animation: fadeIn 0.3s ease-in;
    }

    @keyframes fadeIn {
      0% { opacity: 0; transform: scale(0.5);}
      100% { opacity: 1; transform: scale(1);}
    }

    #message {
      font-size: 1.5rem;
      margin-bottom: 15px;
      font-weight: bold;
      text-shadow: 1px 1px 4px rgba(0,0,0,0.5);
      transition: color 0.3s;
    }

    button {
      padding: 12px 30px;
      font-size: 1.1rem;
      border: none;
      border-radius: 12px;
      cursor: pointer;
      background: #00bfff;
      color: #fff;
      box-shadow: 0 5px 15px rgba(0,0,0,0.3);
      transition: transform 0.2s, background 0.3s;
    }

    button:hover {
      background: #0099cc;
      transform: scale(1.05);
    }

    .winning-cell {
      background-color: #ffeb3b !important;
      color: #000 !important;
      transform: scale(1.3);
      transition: background-color 0.3s, transform 0.3s, color 0.3s;
      box-shadow: 0 0 15px #ffeb3b;
    }
  </style>
</head>
<body>
  <h1>Blue Tic Tac Toe</h1>
  <div class="board" id="board">
    <div class="cell" data-index="0"></div>
    <div class="cell" data-index="1"></div>
    <div class="cell" data-index="2"></div>
    <div class="cell" data-index="3"></div>
    <div class="cell" data-index="4"></div>
    <div class="cell" data-index="5"></div>
    <div class="cell" data-index="6"></div>
    <div class="cell" data-index="7"></div>
    <div class="cell" data-index="8"></div>
  </div>
  <div id="message"></div>
  <button id="restartBtn">Restart Game</button>

  <script>
    const cells = document.querySelectorAll('.cell');
    const message = document.getElementById('message');
    const restartBtn = document.getElementById('restartBtn');

    let currentPlayer = 'X';
    let boardState = ['', '', '', '', '', '', '', '', ''];
    let gameActive = true;

    const winningConditions = [
      [0,1,2],[3,4,5],[6,7,8],
      [0,3,6],[1,4,7],[2,5,8],
      [0,4,8],[2,4,6]
    ];

    function handleCellClick(e) {
      const cell = e.target;
      const index = cell.getAttribute('data-index');
      if(boardState[index] !== '' || !gameActive) return;

      boardState[index] = currentPlayer;
      cell.textContent = currentPlayer;
      cell.classList.add(currentPlayer);

      checkWinner();
    }

    function checkWinner() {
      let roundWon = false;
      let winningCells = [];

      for(let condition of winningConditions){
        const [a,b,c] = condition;
        if(boardState[a] && boardState[a] === boardState[b] && boardState[a] === boardState[c]){
          roundWon = true;
          winningCells = [a,b,c];
          break;
        }
      }

      if(roundWon){
        message.textContent = `Player ${currentPlayer} wins!`;
        message.style.color = currentPlayer === 'X' ? '#00ffff' : '#ff4da6';
        gameActive = false;
        winningCells.forEach(i => cells[i].classList.add('winning-cell'));
        return;
      }

      if(!boardState.includes('')){
        message.textContent = "It's a draw!";
        message.style.color = '#fff';
        gameActive = false;
        return;
      }

      currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
      message.textContent = `Player ${currentPlayer}'s turn`;
      message.style.color = currentPlayer === 'X' ? '#00ffff' : '#ff4da6';
    }

    function restartGame(){
      boardState = ['', '', '', '', '', '', '', '', ''];
      cells.forEach(cell => {
        cell.textContent = '';
        cell.classList.remove('X','O','winning-cell');
      });
      currentPlayer = 'X';
      gameActive = true;
      message.textContent = `Player ${currentPlayer}'s turn`;
      message.style.color = '#00ffff';
    }

    cells.forEach(cell => cell.addEventListener('click', handleCellClick));
    restartBtn.addEventListener('click', restartGame);

    message.textContent = `Player ${currentPlayer}'s turn`;
    message.style.color = '#00ffff';
  </script>
</body>
</html>
