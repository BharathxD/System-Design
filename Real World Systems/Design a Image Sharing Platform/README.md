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

![Image tag with url](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Design%20a%20Image%20Sharing%20Platform/img-tag.png)

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

`https://gravatar.com/{hash(email)}` → Security (Protecting Personally Identifiable Information)

```tsx
if hash("bharath@example.com") = 'Ti09j98j'
```

```html
<img src="https://gravatar.com/Ti09j98j" />
```

This URL will display your current profile picture.

### Requirements

---

- Users can upload multiple pictures.
- Users can designate one as active.
- The active picture should be returned as part of the response.

### Brainstorm

---

- Schema Design

### Schema

---

| Users |
| ----- |
| id    |
| email |
| hash  |

We'll be adding a new column in the upcoming section. But why do we need the 'hash' column? Let's examine the first query below.

| Photos    |
| --------- |
| id        |
| user_id   |
| is_active |

### Query Optimization

---

The following SQL query is highly inefficient and costly. The WHERE clause requires computation and cannot take advantage of indexes.

```sql
SELECT * FROM photos
JOIN users
ON photos.user_id = user.id
WHERE HASH(email) = ?
```

So, what's the more efficient query?

```sql
SELECT * FROM photos
JOIN users
ON photos.user_id = user.id
WHERE users.hash = ?
```

**Updating the photo to mark it active**

```sql
START TRANSACTION;

UPDATE photos
SET is_active = false
WHERE is_active = true
AND user_id = ?;

UPDATE photos
SET is_active = true
WHERE user_id = ?
AND id = ?
AND is_active = false;

UPDATE users
SET active_photo_id = ....
WHERE user_id = ?;

COMMIT;
```
