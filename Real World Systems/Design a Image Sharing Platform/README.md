# Design a private image-sharing platform

### Agenda

---

- Designing Gravatar
- On-demand Image optimisation
- Tagging photos
- Designing newly unread message indicator

### How Images are served

---

![Demonstration of URL parts](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Design%20a%20Image%20Sharing%20Platform/url.png)

Once the request hits the server

- Reads the URL
- Reads the file from the server at the path (Reads the file from S3)
- Sends the response

![Demonstration of URL parts](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Design%20a%20Image%20Sharing%20Platform/img-tag.png)

```tsx
app.get("/raw/:path", async (req: Request, res: Response) => {
  try {
    const path = req.params.path;
    const raw_bytes = await s3.read(path, process.env.BUCKET);
    res.send(raw_bytes);
  } catch (error) {
    res.status(500).send("Internal Server Error");
  }
});
```

### Let’s build Gravatar

---

**What is gravatar?**

Gravatar is your single embeddable URL for profile picture

`https://gravatar.com/{hash(email)}` → Security (PII)

```tsx
if hash("bharath@example.com") = 'Ti09j98j'
```

```html
<img src="https://gravatar.com/Ti09j98j" />
```

Renders the current profile picture

### Requirements

---

- User can upload multiple pictures
- Users can mark one as active
- Active one should be returned as part of the response

### Brainstorm

---

- Schema

### Schema

---

| Users |
| ----- |
| id    |
| email |
| hash  |

- We will add a new column at the coming section
- Hash is derivable right? Yes it is but look at the first query given below

| Photos    |
| --------- |
| id        |
| user_id   |
| is_active |
