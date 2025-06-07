<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>NeoNova AI (Math Expert Offline)</title>
<style>
  body {
    margin: 0;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    animation: colorShift 10s infinite alternate ease-in-out;
    background-color: black;
    color: white;
    transition: background-color 1s ease, color 1s ease;
  }
  @keyframes colorShift {
    0% { background-color: black; color: white; }
    50% { background-color: white; color: black; }
    100% { background-color: black; color: white; }
  }
  .chat-container {
    max-width: 600px;
    margin: 80px auto;
    padding: 20px;
    background: rgba(255,255,255,0.1);
    border-radius: 10px;
    box-shadow: 0 0 30px rgba(0,0,0,0.4);
  }
  .chat-box {
    height: 350px;
    overflow-y: auto;
    background: rgba(0,0,0,0.15);
    padding: 15px;
    border-radius: 8px;
    margin-bottom: 12px;
    font-size: 16px;
    white-space: pre-wrap;
  }
  .message {
    margin: 8px 0;
  }
  .user {
    color: #1abc9c;
    font-weight: bold;
  }
  .ai {
    color: #e67e22;
    font-weight: bold;
  }
  input[type=text] {
    width: 70%;
    padding: 10px;
    font-size: 16px;
    border-radius: 6px;
    border: 1px solid #ccc;
    outline: none;
  }
  button {
    padding: 10px 15px;
    font-size: 16px;
    border: none;
    background-color: #3498db;
    color: white;
    border-radius: 6px;
    cursor: pointer;
    margin-left: 8px;
  }
  button:hover {
    background-color: #2980b9;
  }
</style>
</head>
<body>
  <div class="chat-container">
    <h2>🤖 NeoNova AI (Math Expert Offline)</h2>
    <div id="chatBox" class="chat-box"></div>
    <input id="userInput" type="text" placeholder="Ask NeoNova any math question..." />
    <button onclick="sendMessage()">Send</button>
  </div>

<script>
  // Helper math functions
  function factorial(n) {
    if (n < 0) return null;
    if (n === 0 || n === 1) return 1;
    let res = 1;
    for (let i = 2; i <= n; i++) res *= i;
    return res;
  }

  function isPrime(num) {
    if (num <= 1) return false;
    if (num <= 3) return true;
    if (num % 2 === 0 || num % 3 === 0) return false;
    for (let i = 5; i * i <= num; i += 6) {
      if (num % i === 0 || num % (i + 2) === 0) return false;
    }
    return true;
  }

  // Main math parser answering many common math questions
  function parseMathQuestion(text) {
    text = text.toLowerCase().trim();

    // Addition
    let addMatch = text.match(/add (\d+) (and|to) (\d+)/);
    if (addMatch) {
      let a = Number(addMatch[1]), b = Number(addMatch[3]);
      return `The result of adding ${a} and ${b} is ${a + b}.`;
    }

    // Subtraction
    let subMatch = text.match(/subtract (\d+) (from) (\d+)/);
    if (subMatch) {
      let a = Number(subMatch[1]), b = Number(subMatch[3]);
      return `The result of subtracting ${a} from ${b} is ${b - a}.`;
    }

    // Multiplication
    let mulMatch = text.match(/multiply (\d+) (by) (\d+)/);
    if (mulMatch) {
      let a = Number(mulMatch[1]), b = Number(mulMatch[3]);
      return `The result of multiplying ${a} by ${b} is ${a * b}.`;
    }

    // Division
    let divMatch = text.match(/divide (\d+) (by) (\d+)/);
    if (divMatch) {
      let a = Number(divMatch[1]), b = Number(divMatch[3]);
      if (b === 0) return "Oops, division by zero is not allowed!";
      return `The result of dividing ${a} by ${b} is ${a / b}.`;
    }

    // Simple math expression: 2 + 2, 5 * 6, etc.
    let simpleExpr = text.match(/^(\d+)\s*([\+\-\*\/\^])\s*(\d+)$/);
    if (simpleExpr) {
      let a = Number(simpleExpr[1]);
      let op = simpleExpr[2];
      let b = Number(simpleExpr[3]);
      switch(op) {
        case '+': return `The result of ${a} + ${b} is ${a + b}.`;
        case '-': return `The result of ${a} - ${b} is ${a - b}.`;
        case '*': return `The result of ${a} × ${b} is ${a * b}.`;
        case '/': 
          if (b === 0) return "Oops, division by zero is not allowed!";
          return `The result of ${a} ÷ ${b} is ${a / b}.`;
        case '^': return `The result of ${a} to the power of ${b} is ${Math.pow(a,b)}.`;
      }
    }

    // Square root
    let sqrtMatch = text.match(/square root of (\d+)/);
    if (sqrtMatch) {
      let n = Number(sqrtMatch[1]);
      return `The square root of ${n} is ${Math.sqrt(n)}.`;
    }

    // Power
    let powerMatch = text.match(/(\d+) to the power of (\d+)/);
    if (powerMatch) {
      let base = Number(powerMatch[1]);
      let exp = Number(powerMatch[2]);
      return `${base} to the power of ${exp} is ${Math.pow(base, exp)}.`;
    }

    // Factorial
    let factMatch = text.match(/factorial of (\d+)/);
    if (factMatch) {
      let n = Number(factMatch[1]);
      let fact = factorial(n);
      if (fact === null) return "Factorial is not defined for negative numbers.";
      return `${n}! (factorial) is ${fact}.`;
    }

    // Percentage
    let percentMatch = text.match(/what is (\d+)% of (\d+)/);
    if (percentMatch) {
      let percent = Number(percentMatch[1]);
      let total = Number(percentMatch[2]);
      let res = (percent / 100) * total;
      return `${percent}% of ${total} is ${res}.`;
    }

    // Prime check
    let primeMatch = text.match(/is (\d+) prime/);
    if (primeMatch) {
      let n = Number(primeMatch[1]);
      return n + (isPrime(n) ? " is a prime number." : " is not a prime number.");
    }

    // Solve simple linear equations of type: solve 2x + 3 = 7
    let solveMatch = text.match(/solve ([\d\s\+\-\*\/x]+)=([\d\s\+\-\*\/]+)/);
    if (solveMatch) {
      try {
        let left = solveMatch[1].replace(/\s+/g, '');
        let right = solveMatch[2].replace(/\s+/g, '');

        // Only supports equations like ax + b = c (linear)
        // Extract coefficient of x
        let xCoeffMatch = left.match(/([\-\+]?\d*)x/);
        if (!xCoeffMatch) return "Sorry, I can only solve simple linear equations like '2x + 3 = 7'.";

        let a = Number(xCoeffMatch[1] || '1');
        // Remove 'ax' part from left expression
        let leftWithoutX = left.replace(/[\-\+]?\d*x/, '');
        // Evaluate constant part on left
        let leftConst = eval(leftWithoutX || '0');
        let rightVal = eval(right);

        let x = (rightVal - leftConst) / a;

        return `The solution for x is ${x}.`;
      } catch(e) {
        return "Sorry, I couldn't solve that equation.";
      }
    }

    // If it reaches here, no recognized math format
    return null;
  }

  // Knowledge base for non-math questions (optional)
  const knowledgeBase = {
    "what is roblox": "Roblox is an online platform where you can play games created by other users or build your own games!",
    "who are you": "I am NeoNova AI, your friendly offline assistant!",
    "hello": "Hello! How can I help you today?",
    "hi": "Hey there! What do you want to know?",
    "how are you": "I'm just code, but thanks for asking! Ready to help you.",
    "thank you": "You're welcome!",
    "thanks": "No problem!",
    "bye": "Goodbye! Have a great day!",
  };

  // Typing animation
  async function typeText(element, text, delay = 30) {
    element.textContent = '';
    for (let i = 0; i < text.length; i++) {
      element.textContent += text[i];
      await new Promise(res => setTimeout(res, delay));
      const chatBox = document.getElementById('chatBox');
      chatBox.scrollTop = chatBox.scrollHeight;
    }
  }

  async function sendMessage() {
    const input = document.getElementById('userInput');
    const chatBox = document.getElementById('chatBox');
    let message = input.value.trim();
    if (!message) return;

    // Show user message
    const userDiv = document.createElement('div');
    userDiv.className = 'message user';
    userDiv.textContent = "You: " + message;
    chatBox.appendChild(userDiv);
    chatBox.scrollTop = chatBox.scrollHeight;

    // Show AI typing placeholder
    const aiDiv = document.createElement('div');
    aiDiv.className = 'message ai';
    chatBox.appendChild(aiDiv);
    chatBox.scrollTop = chatBox.scrollHeight;

    input.value = '';
    input.disabled = true;

    let lowerMsg = message.toLowerCase();

    // Check knowledge base first
    let response = knowledgeBase[lowerMsg];

    // Then check math parser
    if (!response) {
      response = parseMathQuestion(lowerMsg);
    }

    if (!response) {
      response = "Sorry, I don't understand that question yet. Try asking a math question or something simple!";
    }

    await typeText(aiDiv, "NeoNova: " + response);

    input.disabled = false;
    input.focus();
  }

  document.getElementById('userInput').addEventListener('keydown', e => {
    if (e.key === 'Enter') sendMessage();
  });
</script>
</body>
</html>

