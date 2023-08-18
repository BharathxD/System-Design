# Important TCP Concepts

**Some important properties of TCP are**

- TCP connection requires 3-Way handshake (for Setup/Start)
- TCP connection requires 2-Way handshake (for Teardown/Termination)
- TCP connectiono does not break immediately after data is exchanged
  - It breaks because of the network interruption
  - It breaks because server/client initiated it
- Hence, connection remains open… almost forever

**How do the client and server communicate?**

- There is a protocol called, HTTP (Hyper Text Transfer Protocol) which helps establish communication between two servers

## HTTP (Hyper Text Transfer Protocol)

---

HTTP is a format that client and server understands

(You can also define your own format, and make your client send data in it, your server passes it and processes it)

### Properties of HTTP 1.1

There are many versions of it - `**HTTP 1.1**` / `**HTTP 2**` / `**HTTP 3**`

HTTP 1.1 is most commonly used

- For client and server to talk over HTTP 1.1, they need to establish TCP connection (It can also happen in UDP, but in most cases it is TCP as it is reliable)
- Connection is typically terminated once the response is complete (Once the response is sent to the client)
- Almost new connection for every request/response (Although it’s expensive as new TCP connections need 3-Way handshake)
- An HTTP header called `**connection:keep-alive**`, make sure that connection stay established (As long as the server supports the header)

## Websockets

---

Websockets meant to do **`bi-directional communication`**

Key-feature: Server can proactively send data to the client, without client asking for it

- Because there is no need for setting up TCP, very single time, we get really low latency in communication

Use case: anywhere you need “Real-time” (or) “Low-latency”, communication for our end user over the internet think about websockets

eg: Chat App, Realtime likes on a live stream App, stock market
