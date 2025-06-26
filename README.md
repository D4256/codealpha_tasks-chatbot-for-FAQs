# codealpha_tasks-chatbot-for-FAQs 
<!DOCTYPE html>
<html>
<head>
  <title>FAQ Chatbot</title>
  <link rel="stylesheet" href="/static/style.css">
</head>
<body>
  <div class="chat-container">
    <h2>FAQ Chatbot</h2>
    <div id="chat-box"></div>
    <input type="text" id="user-input" placeholder="Ask a question...">
    <button onclick="sendMessage()">Send</button>
  </div>

  <script>
    function sendMessage() {
      let input = document.getElementById("user-input");
      let message = input.value;
      if (message === "") return;
      
      let chatBox = document.getElementById("chat-box");
      chatBox.innerHTML += `<div class="user-msg">You: ${message}</div>`;
      input.value = "";

      fetch("/get", {
        method: "POST",
        headers: {"Content-Type": "application/json"},
        body: JSON.stringify({ message: message })
      })
      .then(response => response.json())
      .then(data => {
        chatBox.innerHTML += `<div class="bot-msg">Bot: ${data.response}</div>`;
        chatBox.scrollTop = chatBox.scrollHeight;
      });
    }
  </script>
</body>
</html>


