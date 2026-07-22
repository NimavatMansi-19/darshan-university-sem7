# Lab 25: Implement Basic Chat Application using Socket.IO

## Aim
Implement a basic real-time chat application using **Socket.IO** where multiple users can send and receive messages instantly.

---

# What is Socket.IO?

**Socket.IO** is a JavaScript library that enables **real-time, bidirectional communication** between the client (browser) and the server.

Unlike HTTP, where the client sends a request and waits for a response, Socket.IO maintains a **persistent connection** so both the client and server can send data at any time.

### Example

Think of **WhatsApp**.

- You send a message.
- The other user receives it instantly.
- No page refresh is required.

This real-time communication is achieved using **WebSockets**, with Socket.IO providing additional features such as automatic reconnection and fallback transports.

---

# Why Do We Need Socket.IO?

Normally, web applications use the **HTTP request-response model**.

```
Client -------- Request --------> Server
Client <------- Response -------- Server
```

If new data (such as a chat message) arrives, the browser must repeatedly ask the server for updates.

This process is called **Polling**.

### Problems with Polling

- Generates unnecessary network requests
- Slower response time
- Wastes bandwidth
- Poor user experience

Socket.IO solves this by maintaining a persistent connection.

```
Client <=======================> Server
        Continuous Connection
```

The server can now send data instantly whenever an event occurs.

---

# Features of Socket.IO

- Real-time communication
- Bidirectional (two-way) communication
- Event-based architecture
- Automatic reconnection
- Cross-browser support
- Supports broadcasting
- Supports rooms and namespaces
- Easy integration with Express.js

---

# Real-Life Applications

- WhatsApp
- Facebook Messenger
- Discord
- Google Docs
- Live Cricket Score
- Online Multiplayer Games
- Stock Market Applications
- Notification Systems

---

# Installation

## Step 1: Create Project

```bash
mkdir chat-app
cd chat-app
npm init -y
```

## Step 2: Install Required Packages

```bash
npm install express socket.io
```

---

# Project Structure

```
chat-app
│
├── server.js
│
├── public
│   ├── index.html
│   ├── script.js
│   └── style.css
│
└── package.json
```

---

# How Socket.IO Works

```
Client A
     │
     │
 Socket
     │
     │
Socket.IO Server
     │
     │
 Socket
     │
Client B
```

### Working Process

1. Client connects to the server.
2. User types a message.
3. Client sends the message using an event.
4. Server receives the event.
5. Server broadcasts the message.
6. All connected clients receive and display the message.

---

# Basic Workflow

```
User Types Message
        │
        ▼
Client emits event
        │
        ▼
Server receives event
        │
        ▼
Server broadcasts event
        │
        ▼
All connected clients receive message
        │
        ▼
Message displayed
```

---

# Important Socket.IO Methods

## Server Side

### Creating Socket.IO Server

```javascript
const express = require("express");
const app = express();

const http = require("http").createServer(app);

const io = require("socket.io")(http);
```

---

### Detect Client Connection

```javascript
io.on("connection", (socket) => {
    console.log("User Connected");
});
```

Runs whenever a new client connects.

---

### Receive Data

```javascript
socket.on("chat message", (msg) => {
    console.log(msg);
});
```

Listens for messages sent by clients.

---

### Send Message to Everyone

```javascript
io.emit("chat message", msg);
```

Broadcasts the message to all connected users.

---

### Detect Disconnection

```javascript
socket.on("disconnect", () => {
    console.log("User Disconnected");
});
```

Runs when a client disconnects.

---

# Client Side

## Connect to Server

```javascript
const socket = io();
```

---

## Send Data

```javascript
socket.emit("chat message", message);
```

---

## Receive Data

```javascript
socket.on("chat message", (msg) => {
    console.log(msg);
});
```

---

# Socket.IO Events

Socket.IO works using **events**.

```
Client

socket.emit("chat message")

        │

        ▼

Server

socket.on("chat message")

        │

        ▼

Server

io.emit("chat message")

        │

        ▼

Clients

socket.on("chat message")
```

---

# Chat Application

## Step 1: Server (server.js)

```javascript
const express = require("express");
const app = express();

const http = require("http").createServer(app);

const io = require("socket.io")(http);

app.use(express.static("public"));

io.on("connection", (socket) => {

    console.log("User Connected");

    socket.on("chat message", (msg) => {

        io.emit("chat message", msg);

    });

    socket.on("disconnect", () => {

        console.log("User Disconnected");

    });

});

http.listen(3000, () => {

    console.log("Server Running on Port 3000");

});
```

---

## Step 2: HTML (index.html)

```html
<!DOCTYPE html>
<html>
<head>

    <title>Chat Application</title>

    <link rel="stylesheet" href="style.css">

</head>

<body>

<div class="chat">

    <ul id="messages"></ul>

    <input
        id="message"
        type="text"
        placeholder="Enter message">

    <button onclick="sendMessage()">
        Send
    </button>

</div>

<script src="/socket.io/socket.io.js"></script>

<script src="script.js"></script>

</body>
</html>
```

---

## Step 3: Client (script.js)

```javascript
const socket = io();

function sendMessage(){

    const input = document.getElementById("message");

    socket.emit("chat message", input.value);

    input.value = "";

}

socket.on("chat message", function(msg){

    const li = document.createElement("li");

    li.textContent = msg;

    document.getElementById("messages").appendChild(li);

});
```

---

## Step 4: CSS (style.css)

```css
body{

    font-family: Arial;

    display:flex;

    justify-content:center;

    margin-top:50px;

}

.chat{

    width:350px;

}

#messages{

    height:300px;

    border:1px solid gray;

    overflow:auto;

    list-style:none;

    padding:10px;

}

input{

    width:75%;

    padding:10px;

}

button{

    padding:10px;

}
```



# Message Flow

```
Client A
    │
    │ socket.emit()
    ▼
Server
    │
    │ io.emit()
    ▼
───────────────────────────────
│                             │
▼                             ▼
Client A                  Client B

Display Message        Display Message
```

---

# Advantages of Socket.IO

- Real-time communication
- Faster than repeated HTTP requests
- No page refresh required
- Easy implementation
- Automatic reconnection
- Cross-platform support
- Supports broadcasting and private messaging

---

# Disadvantages

- Requires a persistent server connection
- Uses more server resources than HTTP
- Slightly harder to scale for very large applications

