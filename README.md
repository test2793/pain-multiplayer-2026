# pain-multiplayer-2026

it paint for android multiplayer making on java and c++ on android!! i dont have pc xD 

server part making on nodejs i do tutorial later is easy


# server part tutorial

server requirements: nodejs, npm.

after downloading npm you must do " npm install express socket.io "
next add server.js script like " nano server.js " this script: 

const express = require('express');
const http = require('http');
const { Server } = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = new Server(server);

let drawingHistory = [];

io.on('connection', (socket) => {
    console.log('Connected!');

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

server.listen(3000, '0.0.0.0', () => {
    console.log('Сервер запущен!');
});

and do " node server.js "
