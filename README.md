# AI-CHATBOT-IMPLEMENTATION

## AIM:
To design and implement a simple chatbot application using Node.js and Socket.io that enables real-time communication between a user and the system, where the chatbot responds automatically based on predefined rules or patterns.

## APPARATUS / REQUIREMENTS:

Hardware: Computer with Internet access
Software: Node.js, VS Code or any text editor, Web browser
Libraries: Express.js, Socket.io

## ALGORITHM:
1.Start Node.js and create a project folder.
2.Initialize the project using npm init -y.
3.Install dependencies using npm install express socket.io.
4.Write the chatbot server and client code in server.js.
5.Run the application using node server.js.
6.Open the chatbot in a web browser at http://localhost:3000.
7.Interact with the chatbot to test real-time message exchange

## PROGRAM:

## NAME: Viswanadham Venkata Sai Sruthi
## REG NO: 212223100061

## 1. Create a new folder
```
mkdir simple-chatbot
cd simple-chatbot
```
## 2. Create a new file called server.js
```
/* 
 Simple Node.js Chatbot (server + frontend)
 ------------------------------------------
 To run locally:

 1️⃣  Run these commands:
     npm init -y
     npm install express socket.io

 2️⃣  Start the server:
     node server.js

 3️⃣  Open your browser and visit:
     http://localhost:3000

 Then you’ll see a working chatbot page.
*/

const express = require('express');
const http = require('http');
const { Server } = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = new Server(server);

// Serve the HTML chatbot UI
app.get('/', (req, res) => {
  res.send(`<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Simple Chatbot</title>
<style>
  body{font-family:Arial,Helvetica,sans-serif;margin:0;background:#f4f7fb}
  .app{max-width:800px;margin:36px auto;background:#fff;border-radius:12px;
       box-shadow:0 6px 24px rgba(20,20,50,.08);overflow:hidden}
  header{padding:16px 20px;border-bottom:1px solid #eee}
  .messages{height:500px;padding:20px;overflow:auto;background:#f9fbff}
  .msg{margin-bottom:12px;display:flex}
  .msg.bot{justify-content:flex-start}
  .msg.user{justify-content:flex-end}
  .bubble{max-width:78%;padding:10px 14px;border-radius:12px;background:#eef2ff}
  .bubble.user{background:#0066ff;color:#fff}
  .composer{display:flex;padding:12px;border-top:1px solid #eee}
  .composer input{flex:1;padding:10px;border-radius:999px;border:1px solid #ddd;margin-right:8px}
  .composer button{padding:10px 14px;border-radius:999px;border:none;background:#0066ff;color:#fff}
</style>
</head>
<body>
<div class="app">
  <header><strong>💬 Simple Chatbot</strong></header>
  <div class="messages" id="messages"></div>
  <div class="composer">
    <input id="input" placeholder="Type a message and press Enter" autofocus />
    <button id="send">Send</button>
  </div>
</div>

<script src="/socket.io/socket.io.js"></script>
<script>
  const socket = io();
  const messagesEl = document.getElementById('messages');
  const inputEl = document.getElementById('input');
  const sendBtn = document.getElementById('send');

  function renderMessage(text, who){
    const wrap = document.createElement('div');
    wrap.className = 'msg ' + who;
    const bubble = document.createElement('div');
    bubble.className = 'bubble ' + (who === 'user' ? 'user' : 'bot');
    bubble.textContent = text;
    wrap.appendChild(bubble);
    messagesEl.appendChild(wrap);
    messagesEl.scrollTop = messagesEl.scrollHeight;
  }

  function sendMessage(){
    const text = inputEl.value.trim();
    if(!text) return;
    renderMessage(text, 'user');
    socket.emit('user_message', text);
    inputEl.value = '';
  }

  sendBtn.addEventListener('click', sendMessage);
  inputEl.addEventListener('keydown', (e) => { if(e.key === 'Enter'){ sendMessage(); } });

  socket.on('bot_message', (text) => {
    renderMessage(text, 'bot');
  });
</script>
</body>
</html>`);
});

// Handle chat logic
io.on('connection', (socket) => {
  console.log('Client connected', socket.id);
  socket.emit('bot_message', "👋 Hello! I'm a simple chatbot. Type 'help' to see options.");

  socket.on('user_message', (msg) => {
    const reply = generateBotReply(msg);
    setTimeout(() => socket.emit('bot_message', reply), 600);
  });

  socket.on('disconnect', () => {
    console.log('Client disconnected', socket.id);
  });
});

// Simple rule-based bot
function generateBotReply(message) {
  const msg = message.toLowerCase();

  if (msg.includes('hello') || msg.includes('hi')) return "Hi there! 😊 How can I help you?";
  if (msg.includes('your name')) return "I'm Chatty, a simple demo chatbot!";
  if (msg.includes('how are you')) return "I'm just code, but I'm feeling great! 💻";
  if (msg.includes('help')) return "Try asking about 'time', 'joke', or 'weather'.";
  if (msg.includes('joke')) return "😂 Why do programmers hate nature? It has too many bugs!";
  if (msg.includes('time')) return "I can’t tell time yet, but your computer clock can!";
  if (msg.includes('weather')) return "☁️ I can’t fetch weather now, but you can add an API later!";

  return `You said: "${message}". I'm a demo bot — customize me in the code!`;
}

const PORT = process.env.PORT || 3000;
server.listen(PORT, () => console.log(`✅ Server running at http://localhost:${PORT}`));
```
## OUTPUT:
When the program is executed, the chatbot interface opens in a web browser.
The user can type messages, and the chatbot responds instantly with predefined replies.
It demonstrates real-time communication between client and server using Socket.io.

## RESULT:
The chatbot application was successfully developed and executed using Node.js and Socket.io.
It allows real-time, bidirectional message exchange between the user and the system, achieving the aim of the experiment.
