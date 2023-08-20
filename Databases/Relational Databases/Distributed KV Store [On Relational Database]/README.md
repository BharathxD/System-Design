# Distributed KV Store [On Relational Database]

**Requirements**

- Infinitely Scalable
- GET, PUT, DEL, TTL

**Brainstorm**

- Storage
- Optimize Storage
- Insert
- Update
- TTL

### Schema

| key | value | expired_at |
| --- | ----- | ---------- |
| k1  | v2    | ….         |
| k2  | v1    | ….         |

---

**Delete**

```sql
UPDATE store SET expired_at = -1 WHERE key = k
```

**Insert**

```sql
UPSERT INTO store VALUES(key, value, NOW()) (..);
```

`If your database engine supports you can use UPSERT (Postgres Does), REPLACE INTO (MySQL)`
