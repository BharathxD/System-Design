### CDN

---

CDN transparently sits between your user and the origin

![CDN Demonstration](../../Images/Design%20a%20Social%20Media%20Network/cdn.png)

Consider this scenario: Your point of origin is **[https://example.com](https://example.com/)**, and it hosts a variety of content types, such as JSON, Bytes, HTML, Video, Image, and more.

Meanwhile, your Content Delivery Network (CDN) operates under its distinct domain, like **[https://eg.mycdn.net](https://eg.mycdn.net/)**.

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

URL Decomposition: s3://images/`<user_id>`/`<image_id>`

- Linked[In] also does this
- CDNs also do this

<aside>
💡 When the organization decides to switch from, for example, Cloudflare CDN to Amazon CloudFront CDN, changing all the URLs in the tables can be a cumbersome process.

</aside>
<br/>
<aside>
⚠️ Anything that is derivable, don’t store the derived information

</aside>
