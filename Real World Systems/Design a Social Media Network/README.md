# Design a social media network

In this design system, we'll be exploring different aspects of designing a social media network, with a focus on key components like Content Delivery Networks (CDNs), managing bandwidth, optimizing data schema, considering privacy, optimizing images, and understanding how hashtag services work. We'll be diving into the architectural details, optimization strategies, and important things to remember in order to create a social media platform that's efficient and responsive.

### CDN (Content Delivery Network)

---

A content delivery network (CDN) is a geographically distributed group of servers that caches content close to end users. A CDN allows for the quick transfer of assets needed for loading Internet content, including HTML pages, JavaScript files, stylesheets, images, and videos.

![CDN Demonstration](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Design+a+Social+Media+Network/cdn.png)

Consider this scenario: Your point of origin is **[https://example.com](https://example.com/)**, and it hosts a variety of content types, such as JSON, Bytes, HTML, Video, Image, and more.

Meanwhile, your Content Delivery Network (CDN) operates under its distinct domain, like **[https://eg.mycdn.net](https://eg.mycdn.net/)**.

### Bandwidth Concern (Pre-signed URL with expiration)

By default, all Amazon S3 objects are private, only the object owner has permission to access them. However, the object owner may share objects with others by creating a presigned URL. A presigned URL uses security credentials to grant time-limited permission to download objects. The URL can be entered in a browser or used by a program to download the object. The credentials used by the presigned URL are those of the AWS user who generated the URL.

For general information about presigned URLs, see [Working with presigned URLs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-presigned-url.html).

![Presigned url architectural flow](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Design+a+Social+Media+Network/presigned-url.png)

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

![Overall Architecture](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Design+a+Social+Media+Network/overall-arch.png)

### Image Optimizations

---

1. Users belong to different geographies, different network bandwidths, different mobile devices, different processing power
2. So, sending 5MB photo uploaded by your favourite celeb to all the followers is a very bad idea!
3. So we should have different resolutions of photos ready to server to the users depending on their “state”

<aside>
💡 Instead of building our image optimizer service, we can use CDN that gives this feature out-of-the-box

</aside>
<br />

![CDN Link Demonstration example](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Design+a+Social+Media+Network/cdn-link-demonstration.png)

## The Hastag Service

Let's create a hashtag service

### Requirements

Millions of hashtags exist, and let's assume there's a service that notifies us of where it generates 'top' photos for a hashtag.

### Brainstorm

- Storage
- Counting (In the large volumes)
- Inter-service communication (Posts service to Hashtag service ensuring atomicity and partial update)
- Super-fast response times

![UI demonstration](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Design+a+Social+Media+Network/hastag_overview.png)

Given that we are keeping track of only top 100 posts, it’s probably better to store all 100 posts in an array for a particular hashtag

**First Approach (Consistant)**

```js
{
    tag: "sunset",
    total_photos: 1_200_000,
    top_100: ["user_1", "user_2", ....]
}
```

`Fast, less space but needs to talk to posts db`

**Second Approach (Efficient)**

```js
{
    tag: "sunset",
    total_photos: 1_200_000,
    top_100: [
        {
            id,
            ....
        },
        ....
    ]
}
```

`Large space, fast time and do not need to talk to posts db`

It all comes down to the Preferences or SLA

### How do we update total number of photos and top hundred posts?

---

PS: It need not to happen in real-time

![Demonstration on how updateing the count and top 100 posts work](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Design+a+Social+Media+Network/update_count_and_posts.png)

- Reads POST_PUBLISH events from kafka
- Extract all the hashtags from the post
- Creates `n` new events on topic `POST_HASHTAG` from n hosts partitioned by hashtag

<aside>
💡 Note that we are using hash-based partitioning. You may have three partitions, and millions of hashtags are distributed across them (Same goes for kafka with user_id partition)

</aside>

### Overall Architecture

---

![Overall Architecture](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Design+a+Social+Media+Network/overall_arch.png)

### Key Takeaways

---

- Kafka as a glue
- Adaptor Pattern
- Effective batching and counting
- READ and WRITE path optimizations

### Further Optimizations

---

A good way to optimize systems is to identify the `READ PATH` and `WRITE PATH` and optimize them independently

**READ PATH optimizations**

- Key value based access for a key (hashtag) get the value (details)

**WRITE PATH optimizations**

- Ingestion in Kafka
- Reading from Kafka
- Quick in memory counting
