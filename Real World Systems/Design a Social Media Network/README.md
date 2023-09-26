### CDN

---

CDN transparently sits between your user and the origin

![CDN Demonstration](../../Images/Design%20a%20Social%20Media%20Network/cdn.png)

Say, your origin is `https://example.com`, and there is a path that returns some response. JSON, Bytes, HTML, Video, Image, etc.

CDN has it’s own domain, say `https://eg.mycdn.net`

### Bandwidth Concern Pre-signed URL with expiration

---

![Presigned url architectural flow](../../Images/Design%20a%20Social%20Media%20Network/presigned-url.png)

### Schema

---

| Post ❌ |
| ------- |
| user_id |
| caption |
| url     |

Building the table in the manner shown above would be highly suboptimal. Why? Because if something is derivable, there is no need to store it.

**So what’s the better way of building it?**

The user keeps track of the random image ID it got from the image it uses this create the entry in the posts table

| Posts ✅ |
| -------- |
| id       |
| user_id  |
| image_id |
| caption  |
