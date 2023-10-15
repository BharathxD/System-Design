# Design Slack’s real-time communication system [Text]

**Requirements**

- Multiple Users, Multiple Channels
- Users DM on Message Channel
- Real-time chat
- Historical messages can be scrolled through

**Similar Systems**

- Multiplayer Games
- Realtime Chat
- Interactions
- Realtime Polls
- Creator Tools

**Brainstorm**

- Channels
- Messages
- Checkpoints (Membership)
- Realtime Communication

### Database Schema [LLD]

---

| Users |
| ----- |
| id    |
| name  |

| Membership   |
| ------------ |
| user_id      |
| channel_id   |
| [Checkpoint] |
| muted        |
| read_till    |

| Channels       |     |
| -------------- | --- |
| id             |     |
| user_id        |     |
| org            |     |
| name           |     |
| type: group_id |     |

| Messages   |
| ---------- |
| from       |
| channel_id |
| created_at |
| message    |

<aside>
💡 Always create a separate table just like `subscription`, you can also add fields like checkpoint, muted, read_till. Also make sure that it should be a `noun`

</aside>

### WebSockets

---

Every user will have 1 WebSocket connection open with our backend infrastructure and that will be used for anything and everything that happens `real-time`

**Edge Servers**

Because WebSocket are `expensive` and browsers have a **6 Concurrent TCP** connection limit, we have to multiplex all realtime communication on `ONE WEBSOCKET` connection

Hence, we need a fleet of servers (Edge Servers) to whom our end users connect over WebSocket [WS]

<aside>
💡 If any service, such as chat or notifications, needs to engage with users in real-time, the data will be routed through these edge servers. Avoid creating multiple WebSocket connections, as this could potentially slow down the system.

</aside>

### Overall Architecture

---

![Overall Architecture](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Slack’s%20real-time%20communication%20system%20[Text]/slack.png)
