# Socket.IO Chat Application Notes

## What is Socket?

A **Socket** is a communication endpoint that allows **two-way (bidirectional)** communication between a client and a server.

Unlike HTTP, where:

```
Client ---> Request ---> Server
Client <--- Response <--- Server
```

Socket allows continuous communication:

```
Client <-------------------------> Server
        Send & Receive Anytime
```

The connection remains open until either the client or server disconnects.

---

# What is Socket.IO?

**Socket.IO** is a JavaScript library that provides real-time communication between client and server.

It works on top of **WebSocket** and automatically falls back to other protocols if WebSocket is unavailable.

### Installation

Server

```bash
npm install express socket.io
```

Client

```bash
npm install socket.io-client
```

---

# Advantages

- Real-time communication
- Two-way communication
- Low latency
- Automatic reconnection
- Event-based programming
- Works across browsers

---

# Project Architecture

```
                Socket Connection

+--------------------+        +----------------------+
|                    |        |                      |
| React Application  |<-----> | Express + Socket.IO |
|    (Client)        |        |      (Server)       |
|                    |        |                      |
+--------------------+        +----------------------+
```

---

# Code Flow

```
React Starts
      │
      ▼
Creates Socket Connection
      │
      ▼
Connects to Express Server
      │
      ▼
Server Generates Socket ID
      │
      ▼
Connection Established
      │
      ▼
User Types Message
      │
      ▼
socket.emit("message")
      │
      ▼
Server Receives Event
      │
      ▼
io.emit("receive-message")
      │
      ▼
Every Connected Client Receives Message
      │
      ▼
React Updates UI
```

---

# Server Code Explanation

## Import Modules

```javascript
import express from "express";
import { Server } from "socket.io";
```

### express

Creates HTTP server.

### Server

Socket.IO server class.

---

## Create Express App

```javascript
const app = express();
```

Creates Express application.

---

## Start HTTP Server

```javascript
const httpServer = app.listen(2000, () => {
    console.log("Server started");
});
```

Server listens on **Port 2000**.

---

## Attach Socket.IO

```javascript
const io = new Server(httpServer,{
    cors:{
        origin:"http://localhost:5173"
    }
});
```

Socket.IO uses the existing HTTP server.

### Why CORS?

React runs on

```
http://localhost:5173
```

Server runs on

```
http://localhost:2000
```

Different origins cannot communicate unless CORS is enabled.

---

## Listen for Connection

```javascript
io.on("connection",(socket)=>{
```

Runs every time a client connects.

Example

```
Client 1 connects

↓

socket.id = abcd123
```

Another client

```
socket.id = xyz987
```

Each client has a unique Socket ID.

---

## Print Socket ID

```javascript
console.log(socket.id + " connected");
```

Example Output

```
NluZkRpdYo8AXjSRAAAE connected
```

---

## socket.onAny()

```javascript
socket.onAny((event,...args)=>{
    console.log(event);
});
```

Listens to **every event**.

Useful for debugging.

If client sends

```javascript
socket.emit("message","Hello");
```

Output

```
Event : message
Arguments : Hello
```

---

## Receive Message

```javascript
socket.on("message",(msg)=>{
```

Waits for

```javascript
socket.emit("message",...)
```

from client.

---

## Print Message

```javascript
console.log("Client:",msg);
```

Output

```
Client : Hello
```

---

## Broadcast Message

```javascript
io.emit("receive-message",msg);
```

Sends message to **all connected clients**.

Suppose three clients exist.

```
Client A

Client B

Client C
```

A sends

```
Hello
```

Server broadcasts

```
Hello
```

Everyone receives

```
A
B
C
```

including sender.

---

## Disconnect

```javascript
socket.on("disconnect",()=>{
```

Runs when user closes browser or disconnects.

Output

```
abcd123 disconnected
```

---

# Client Code Explanation

## Import

```javascript
import { io } from "socket.io-client";
```

Allows React to connect to Socket.IO server.

---

## Create Socket

```javascript
const socket = useMemo(()=>
    io("http://localhost:2000"),[]);
```

Creates only one socket connection.

### Why useMemo?

Without it

```
Component Re-render

↓

New Socket

↓

New Socket

↓

New Socket
```

Multiple unnecessary connections are created.

Using

```javascript
useMemo()
```

creates only one socket instance.

---

## State

```javascript
const [message,setMessage]=useState("");
```

Stores input message.

Example

```
Hello
```

---

```javascript
const [messages,setMessages]=useState([]);
```

Stores chat history.

Example

```
[
Hello,
How are you?,
Bye
]
```

---

# useEffect()

Runs after component mounts.

---

## Connect Event

```javascript
socket.on("connect",()=>{
```

Runs once connection is established.

Output

```
Connected to server

Socket ID : xyz123
```

---

## Disconnect Event

```javascript
socket.on("disconnect",()=>{
```

Runs when connection closes.

Output

```
Disconnected
```

---

## Receive Message

```javascript
socket.on("receive-message",(msg)=>{
```

Server emits

```javascript
io.emit("receive-message",msg);
```

React receives it.

---

## Update UI

```javascript
setMessages(prev=>[...prev,msg]);
```

Example

Before

```
[
Hello
]
```

Receive

```
How are you?
```

After

```
[
Hello,
How are you?
]
```

---

## Cleanup

```javascript
return ()=>{
```

Removes event listeners.

```javascript
socket.off("connect");
socket.off("disconnect");
socket.off("receive-message");
```

Finally disconnects

```javascript
socket.disconnect();
```

Prevents memory leaks.

---

# sendMessage()

```javascript
socket.emit("message",message);
```

Sends event to server.

Example

```
message = "Hi"
```

Sends

```
Event : message

Data : Hi
```

---

Then

```javascript
setMessage("");
```

Clears input box.

---

# JSX

Input

```javascript
<input
value={message}
onChange={(e)=>setMessage(e.target.value)}
/>
```

User types

```
Hello
```

State updates continuously.

---

Button

```javascript
<button onClick={sendMessage}>
```

Calls

```
sendMessage()
```

---

Display Messages

```javascript
messages.map(...)
```

Displays

```
Hello

How are you?

Bye
```

---

# Complete Message Flow

```
User Types

        │
        ▼

message State Updated

        │
        ▼

Click Send

        │
        ▼

socket.emit("message")

        │
        ▼

Server Receives

socket.on("message")

        │
        ▼

Server Prints Message

        │
        ▼

io.emit("receive-message")

        │
        ▼

All Clients Receive

        │
        ▼

setMessages()

        │
        ▼

React Re-renders

        │
        ▼

Chat Updated
```

---

# Important Socket.IO Methods

| Method | Purpose |
|---------|---------|
| `io()` | Connect to server |
| `new Server()` | Create Socket.IO server |
| `io.on("connection")` | Detect new client |
| `socket.on()` | Listen for one event |
| `socket.onAny()` | Listen for every event |
| `socket.emit()` | Send event to server |
| `io.emit()` | Send event to all clients |
| `socket.off()` | Remove event listener |
| `socket.disconnect()` | Close connection |

---

# Difference Between socket.emit() and io.emit()

### socket.emit()

```
Client
   │
   ▼
Server
```

Sends data from one socket.

Example

```javascript
socket.emit("message","Hello");
```

---

### io.emit()

```
          Server
      /      |      \
     ▼       ▼       ▼
Client A  Client B Client C
```

Broadcasts data to **every connected client**.

---
### Backend/Server.js
```
import express from "express";
import { Server } from "socket.io";
const app=express();
const httpServer = app.listen(2000, () => {
    console.log("Server started on port 2000");
});
const io = new Server(httpServer, {
    cors: {
        origin: "http://localhost:5173"
    }
});
io.on("connection",(socket)=>{
    console.log(socket.id+" connected")
        socket.on("message", (msg) => {
            console.log("Client:", msg);
        });
 socket.on("disconnect", () => {
        console.log(socket.id + " disconnected");
    });


})

```
### Frontend/ChatApp.jsx
```
import { useEffect, useMemo, useState } from "react";
import { io } from "socket.io-client";

function ChatApp() {

    
    const socket = useMemo(() => io("http://localhost:2000"), []);

    const [message, setMessage] = useState("");
    const [messages, setMessages] = useState([]);

    useEffect(() => {

        socket.on("connect", () => {
            console.log("Connected to server");
            console.log("Socket ID:", socket.id);
        });

        // Disconnected
        socket.on("disconnect", () => {
            console.log("Disconnected from server");
        });

        // Receive message from server
        socket.on("receive-message", (msg) => {
            console.log("Received:", msg);
            setMessages((prev) => [...prev, msg]);
        });

        
        return () => {
            socket.off("connect");
            socket.off("disconnect");
            socket.off("receive-message");
            socket.disconnect();
        };

    }, [socket]);

    const sendMessage = () => {
    console.log("Connected?", socket.connected);
    console.log("Socket ID:", socket.id);

    socket.emit("message", message);

    console.log("Sent:", message);

    setMessage("");
};

    return (
        <div style={{ padding: "20px" }}>
            <h2>Socket Chat</h2>

            <input
                type="text"
                placeholder="Enter message"
                value={message}
                onChange={(e) => setMessage(e.target.value)}
            />

            <button onClick={sendMessage}>
                Send
            </button>

            <hr />

            {messages.map((msg, index) => (
                <p key={index}>{msg}</p>
            ))}
        </div>
    );
}

export default ChatApp;
```
# Key Points to Remember

- Socket provides persistent two-way communication.
- Socket.IO is event-driven.
- Every connected client gets a unique `socket.id`.
- `socket.emit()` sends an event.
- `socket.on()` receives an event.
- `io.emit()` broadcasts to all connected clients.
- `useMemo()` prevents multiple socket connections on re-render.
- `useEffect()` is used to register socket events after the component mounts.
- Cleanup using `socket.off()` and `socket.disconnect()` avoids memory leaks.
- React updates the UI whenever a new message is added to the `messages` state.
