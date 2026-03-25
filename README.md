# 🎨 pain-multiplayer-2026

Multiplayer paint application for Android. Developed entirely on mobile using Java and C++! 📱

> **Note:** This project is being built without a PC. Proof that you only need a phone and a dream (and maybe Termux).

## 🚀 Server Part Setup

The server handles real-time drawing synchronization using **Node.js** and **Socket.io**.

### Requirements
* **Node.js** & **npm**

### Installation
1. Install the necessary dependencies:
   ```bash
   npm install express socket.io

2. Create the server file:
   ```bash
   nano server.js

3. Paste the following script into server.js:
   ```javascript
   const express = require('express');
   const http = require('http');
   const { Server } = require('socket.io');

   const app = express();
   const server = http.createServer(app);
   const io = new Server(server);

   let drawingHistory = [];

   io.on('connection', (socket) => {
    console.log('User Connected!');

    socket.emit('history', drawingHistory);

    socket.on('draw_step', (data) => {
        drawingHistory.push(data);
        socket.broadcast.emit('draw_step', data);
    });

    socket.on('clear', () => {
        drawingHistory = [];
        io.emit('clear');
    });
   });

   const PORT = 3000;
   server.listen(PORT, '0.0.0.0', () => {
    console.log(`Server is running on port ${PORT}`);
   });

4. Run the Server
   ```bash
   node server.js
   
