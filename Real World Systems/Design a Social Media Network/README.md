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

### Privacy

---

1. Is Instagram's private photo feature truly private? Not quite.
2. Each photo URL is accompanied by a timed signature within the URL.
3. When the URL is rendered via an IMG tag, the request is directed to a Content Delivery Network (CDN).
4. The CDN validates the request by utilizing the attached keys and verifying the validity and expiration status of the certificates.
5. It's important to note that the URL has a limited lifespan due to the short duration of the certificates.

![Overall Architecture](../../Images/Design%20a%20Social%20Media%20Network/overall-arch.png)

### Image Optimizations

---

1. Users belong to different geographies, different network bandwidths, different mobile devices, different processing power
2. So, sending 5MB photo uploaded by your favourite celeb to all the followers is a very bad idea!
3. So we should have different resolutions of photos ready to server to the users depending on their “state”

<aside>
💡 Instead of building our image optimizer service, we can use CDN that gives this feature out-of-the-box

</aside>
<br />

![CDN Link Demonstration example](../../Images/Design%20a%20Social%20Media%20Network/cdn-link-demonstration.png)
