# Design Horizontally Scalable Chat Application

In this design system, I have leveraged Redis pub/sub and multiple web socket servers to create a chat application that can scale both vertically and horizontally. 📈

Here's a simplified breakdown of its functionality

1️⃣ Three Web Socket Servers are deployed, each subscribing to a Redis pub/sub.

2️⃣ The web socket server quickly publishes incoming messages to the Redis pub/sub.

3️⃣ The other connected web socket servers efficiently receive and emit the messages to all connected clients, ensuring real-time delivery. ⚡️

So, whenever there are two users who are connected to two different web-socket servers, they can still maintain real-time communication by leveraging the help of this Publish-Subscribe pattern.

This design system empowers chat applications to handle increased traffic and seamlessly scale horizontally. 🌐🚀

Check the code [here](https://github.com/BharathxD/Realtime-Chat-Engine-with-Horizontal-Scalability)
