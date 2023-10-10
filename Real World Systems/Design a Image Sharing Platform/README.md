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

**Getting the active photo**

```sql
SELECT * FROM photos
JOIN users
ON photos.user_id = users.id
WHERE users.hash = ?
AND photos.is_active = true
```

**How can we make the above query much better? (As this query is frequently used)**

Add a column named `active_photo_id`

| Users           |
| --------------- |
| ….              |
| active_photo_id |

**But then we need to update the other queries?** `YES`

```sql
--- To set the active photo
UPDATE user SET active_photo_id = ? WHERE id = ?
--- To get the active photo
SELECT * FROM photos
WHERE id = (
  SELECT active_photo_id
  FROM users WHERE id = ?
);
```

### Let’s get going by creating an API Server

---

![Uploading a photo api.gravatar.com](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Design%20a%20Image%20Sharing%20Platform/api-server.png)

Uploading a photo api.gravatar.com

**Prepare for upload**

- User requests photo upload service for a new upload
- Photo upload service generates a random id
- Photo upload service generates signed URL for `S3://gravatar/{user_id}/{random_photo_id}`
- Photo upload service sends signed URL to user

**Upload Photo**

User uploads photo to S3 using pre-signed urls (Learn in-depth about it [here](https://github.com/BharathxD/System-Design/tree/master/Real%20World%20Systems/Design%20a%20Social%20Media%20Network))

**Render the active photo**

`https://api.gravatar.com/photos/{hash}`

- Get hash from the URL
- Get active photo id from the database

```sql
SELECT * FROM phtos
JOIN users
WHERE users.hash = {hash}
AND is_active = true;
```

- Read the file from S3
  `S3://gravatar-images/{user-id}/{photo_id}`

- Return the response

```sql
<img src="https://api.gravatar.com/photos/Ti09j98j"/>
```

### Let’s use CDN to serve it

1. Configure a CDN with `gravatar.com` which points to our actual api server `api.gravatar.com/photos`
2. `https://gravatar.com/hash` — Cache Miss? → `https://api.gravatar.com/photos/hash`

### Overall architecture for Gravatar

---

![overall_arch.png](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Design%20a%20Image%20Sharing%20Platform/overall_arch.png)
