---
date: 2017-09-28 00:00:00
title: "Winsock and Websocket video transmitting based on OpenCV"
category: "Coding"
tag: ["Winsock", "Websocket"]
---

This [repo](https://github.com/joshua19881228/windowsMatTrans) implemented transmitting cv::Mat via winsock to front-end and display the image using websocket.

## Server Code ##

The server code is implemented in `websocket_server_c`, which is written in C++ and based on winsock2 on Windows. The server code first construct handshakes and a connection with the client based on TCP protocal. As long as the connection being set up, a video is transmitted to the front-end frame by frame. The frames are extracted using OpenCV. And the frames are encoded in JPEG format first and then encoded to string using base64.

Note that, many examples of socket sending messages ignored the steps of construct connectiong for websocket, which is implemented in this repo.

## Clinet Code ##

The clinet code is a django project in `websocket_client_django`. The only function is to receive the messages from server end and display the frames on web.

## Notice ##

This code is just a demo for using socket in C++ and web. THere must be better way for live video streaming. 
