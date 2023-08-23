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

| Channels       |
| -------------- | ------- |
| id             |
| org            |
| name           |
| type: group_id | user_id |

| Messages   |
| ---------- |
| from       |
| channel_id |
| created_at |
| message    |

<aside>
💡 Always create a separate table just like `subscription`, you can also add fields like checkpoint, muted, read_till. Also make sure that it should be a `noun`

</aside>