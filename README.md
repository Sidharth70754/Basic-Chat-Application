# Basic-Chat-Application
This is a 2nd project of Code Clause intern 

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Basic Chat Application</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background-color: #f4f4f4;
    }

    #chat-box {
      width: 300px;
      height: 400px;
      border: 1px solid #ccc;
      border-radius: 5px;
      overflow-y: scroll;
      padding: 10px;
    }

    #message-input {
      width: calc(100% - 20px);
      margin-top: 10px;
      padding: 5px;
      border: 1px solid #ccc;
      border-radius: 3px;
    }

    #send-btn {
      width: calc(100% - 20px);
      margin-top: 10px;
      padding: 5px;
      background-color: #007bff;
      color: white;
      border: none;
      border-radius: 3px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <div id="chat-box"></div>
  <input type="text" id="message-input" placeholder="Type a message...">
  <button id="send-btn">Send</button>

  <script>
    const chatBox = document.getElementById('chat-box');
    const messageInput = document.getElementById('message-input');
    const sendButton = document.getElementById('send-btn');

    const ws = new WebSocket('wss://echo.websocket.org');

    ws.onopen = () => {
      console.log('Connected to WebSocket server');
    };

    ws.onerror = (error) => {
      console.error('WebSocket error:', error);
    };

    ws.onmessage = (event) => {
      const message = event.data;
      appendMessage(message, 'received');
    };

    sendButton.addEventListener('click', () => {
      const message = messageInput.value;
      if (message.trim() !== '') {
        ws.send(message);
        appendMessage(message, 'sent');
        messageInput.value = '';
      }
    });

    function appendMessage(message, type) {
      const messageElement = document.createElement('div');
      messageElement.classList.add('message', type);
      messageElement.innerText = message;
      chatBox.appendChild(messageElement);

      // Scroll to the bottom of the chat box
      chatBox.scrollTop = chatBox.scrollHeight;
    }
  </script>
</body>
</html>

