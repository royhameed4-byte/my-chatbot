<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AI Chatbot</title>

<style>
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #0f172a;
  color: white;
  display: flex;
  flex-direction: column;
  height: 100vh;
}

header {
  padding: 15px;
  text-align: center;
  background: #020617;
  font-size: 18px;
  font-weight: bold;
}

#chatBox {
  flex: 1;
  padding: 20px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
}

.msg {
  max-width: 70%;
  padding: 12px;
  margin: 8px 0;
  border-radius: 10px;
  line-height: 1.4;
}

.user {
  background: #2563eb;
  align-self: flex-end;
}

.bot {
  background: #1e293b;
  align-self: flex-start;
}

#inputArea {
  display: flex;
  padding: 10px;
  background: #020617;
}

input {
  flex: 1;
  padding: 12px;
  border-radius: 6px;
  border: none;
  outline: none;
}

button {
  padding: 12px;
  margin-left: 10px;
  background: #22c55e;
  border: none;
  color: white;
  cursor: pointer;
  border-radius: 6px;
}

button:hover {
  background: #16a34a;
}
</style>
</head>

<body>

<header>🤖 My AI Chatbot</header>

<div id="chatBox"></div>

<div id="inputArea">
  <input id="userInput" placeholder="Type a message..." />
  <button onclick="sendMessage()">Send</button>
</div>

<script>
const chatBox = document.getElementById("chatBox");

function addMessage(text, className) {
  const div = document.createElement("div");
  div.className = "msg " + className;
  div.innerText = text;
  chatBox.appendChild(div);
  chatBox.scrollTop = chatBox.scrollHeight;
}

async function sendMessage() {
  const input = document.getElementById("userInput");
  const message = input.value.trim();

  if (!message) return;

  addMessage(message, "user");
  input.value = "";

  const loading = document.createElement("div");
  loading.className = "msg bot";
  loading.innerText = "Typing...";
  chatBox.appendChild(loading);

  try {
    const res = await fetch("http://localhost:3000/chat", {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify({ message })
    });

    const data = await res.json();
    loading.remove();
    addMessage(data.reply, "bot");

  } catch (err) {
    loading.remove();
    addMessage("Error connecting to server", "bot");
  }
}

document.getElementById("userInput").addEventListener("keypress", function(e) {
  if (e.key === "Enter") sendMessage();
});
</script>

</body>
</html>
