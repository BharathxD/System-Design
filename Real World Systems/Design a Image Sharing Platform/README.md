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