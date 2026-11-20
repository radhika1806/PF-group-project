# PF-group-project

<!DOCTYPE html>
<html>
<head>
<title>Word Guessing Game</title>
<style>
  body { font-family: Arial; background: #121213; color: white; text-align: center; }
  h1 { color: #538d4e; }
  .box { display: flex; justify-content: center; gap: 5px; margin: 5px; }
  .letter {
    width: 50px; height: 50px; border: 2px solid #3a3a3c;
    display: flex; align-items: center; justify-content: center;
    font-size: 28px; font-weight: bold; text-transform: uppercase;
  }
  .G { background: #538d4e; border-color: #538d4e; }
  .Y { background: #b59f3b; border-color: #b59f3b; }
  .GRAY { background: #3a3a3c; border-color: #3a3a3c; }
  input { padding: 12px; font-size: 18px; text-align: center; border-radius: 8px; border: none; }
  button { padding: 12px 25px; font-size: 18px; background: #538d4e; color: white; border: none; border-radius: 8px; cursor: pointer; margin: 5px; }
  button:hover { background: #6aaa64; }
  #message { margin-top: 15px; font-size: 20px; }
</style>
</head>
<body>

<h1>WORD GUESSING GAME</h1>
<p id="instruction">User A, enter secret word:</p>

<input type="password" id="secretInput" placeholder="Secret word">
<button onclick="setSecret()">Set Word</button>

<br><br>
<input type="text" id="guessInput" placeholder="Your guess" style="display:none;">
<button id="guessBtn" onclick="checkGuess()" style="display:none;">Guess</button>

<div id="board"></div>
<div id="message"></div>

<script>
let original = "";
let secretSet = false;

function setSecret() {
  original = document.getElementById("secretInput").value.toLowerCase().trim();
  if(original.length < 3) { alert("Enter at least 3 letters!"); return; }
  secretSet = true;
  document.getElementById("secretInput").style.display = "none";
  document.querySelector("button").style.display = "none";
  document.getElementById("instruction").innerText = "User B, guess the " + original.length + " letter word!";
  document.getElementById("guessInput").style.display = "inline";
  document.getElementById("guessBtn").style.display = "inline";
  document.getElementById("guessInput").maxLength = original.length;
}

function checkGuess() {
  let guess = document.getElementById("guessInput").value.toLowerCase().trim();
  if(guess.length !== original.length) {
    document.getElementById("message").innerText = "Please enter a word with " + original.length + " letters!";
    return;
  }

  // SAME LOGIC AS YOUR C++ CODE
  let result = new Array(original.length).fill(' ');
  for(let i=0; i<original.length; i++) {
    if(guess[i] === original[i]) result[i] = 'G';
  }
  for(let i=0; i<original.length; i++) {
    if(result[i] === 'G') continue;
    for(let j=0; j<original.length; j++) {
      if(guess[i] === original[j] && result[j] !== 'G') {
        result[i] = 'Y'; break;
      }
    }
  }

  // SHOW VISUAL BOXES
  let row = document.createElement("div");
  row.className = "box";
  for(let i=0; i<guess.length; i++) {
    let div = document.createElement("div");
    div.className = "letter " + (result[i]==='G' ? 'G' : result[i]==='Y' ? 'Y' : 'GRAY');
    div.innerText = guess[i];
    row.appendChild(div);
  }
  document.getElementById("board").appendChild(row);
  document.getElementById("guessInput").value = "";

  if(guess === original) {
    document.getElementById("message").innerText = "🎉 Congratulations! User B guessed the word!";
    document.getElementById("guessBtn").disabled = true;
  }
}
</script>
</body>
</html>
