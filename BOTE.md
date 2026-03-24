# System Design — Back of The Envelope (BOTE) Calculations

> A complete reference for FAANG system design interviews. Every formula, every case study, step by step.

---

## Table of Contents

1. [The Cheat Sheet](#1-the-cheat-sheet)
2. [The 8 Core Calculations](#2-the-8-core-calculations)
3. [Bonus Calculations (Senior Rounds)](#3-bonus-calculations-senior-rounds)
4. [What to Calculate Per System Type](#4-what-to-calculate-per-system-type)
5. [The Universal Framework](#5-the-universal-framework)
6. [Case Study — Twitter](#6-case-study--twitter)
7. [Case Study — WhatsApp](#7-case-study--whatsapp)
8. [Case Study — YouTube](#8-case-study--youtube)
9. [Case Study — Uber](#9-case-study--uber)
10. [Case Study — URL Shortener](#10-case-study--url-shortener)
11. [Case Study — BookMyShow](#11-case-study--bookmyshow)
12. [Numbers Worth Memorizing](#12-numbers-worth-memorizing)
13. [Common Mistakes](#13-common-mistakes)

---

## 1. The Cheat Sheet

### Data Sizes

| Unit | Value | Real-world example |
|------|-------|-------------------|
| 1 KB | 1,000 bytes | A short text email |
| 1 MB | 1,000 KB | A compressed photo |
| 1 GB | 1,000 MB | A movie |
| 1 TB | 1,000 GB | A hard drive |
| 1 PB | 1,000 TB | Large data warehouse |

### Number Sizes

| Name | Value | Zeros |
|------|-------|-------|
| 1 Thousand (K) | 1,000 | 3 |
| 1 Million | 1,000,000 | 6 |
| 1 Billion | 1,000,000,000 | 9 |
| 1 Trillion | 1,000,000,000,000 | 12 |

### Time Conversions

| Period | Exact | Rounded (use this) |
|--------|-------|--------------------|
| 1 minute | 60 sec | 60 sec |
| 1 hour | 3,600 sec | 4,000 sec |
| 1 day | 86,400 sec | **100,000 sec** |
| 1 month | 2,592,000 sec | 2.5M sec |
| 1 year | 31,536,000 sec | 30M sec |

> **Golden rule:** Round aggressively. You're estimating order of magnitude, not filing taxes.

---

## 2. The 8 Core Calculations

### 1. Write QPS — How fast data comes IN

**What it tells you:** DB write capacity needed, ingestion pipeline size, whether a single DB handles writes

```
Write QPS = (DAU × writes per user per day) ÷ 86,400

Rounded: ÷ 100,000
```

**Example:**
```
100M DAU, each user posts 1 tweet/day (5% of users)
= 100M × 5% × 1 = 5M writes/day
= 5,000,000 ÷ 100,000 = 50 write QPS
```

**Architecture signal:**
- < 1K QPS → single DB handles it fine
- 1K–10K QPS → consider write replicas
- > 10K QPS → sharding or NoSQL needed

---

### 2. Read QPS — How fast data goes OUT

**What it tells you:** Whether you need cache, read replicas, CDN

```
Read QPS = (DAU × reads per user per day) ÷ 86,400
```

**Example:**
```
100M DAU, each refreshes feed 10x/day
= 100M × 10 = 1B reads/day
= 1,000,000,000 ÷ 100,000 = 10,000 read QPS
```

**Architecture signal:**
- < 1K QPS → single DB
- 1K–10K QPS → add Redis cache
- > 10K QPS → cache is mandatory, consider CDN

---

### 3. Read:Write Ratio — Which side dominates

**What it tells you:** Whether to optimize for reads or writes, cache strategy

```
Ratio = Read QPS ÷ Write QPS
```

**Example:**
```
10,000 read QPS ÷ 50 write QPS = 200:1
```

**What the ratio means:**

| Ratio | System type | Implication |
|-------|-------------|-------------|
| 1:1 | Messaging, chat | Optimize both equally |
| 10:1 | E-commerce | Cache product pages |
| 100:1 | News feed | Heavy caching, pre-compute feeds |
| 200:1 | Twitter | Entire architecture optimizes for reads |

---

### 4. Storage Per Day — Disk burned daily

**What it tells you:** DB type, whether to use S3, sharding timeline

```
Storage/day = writes per day × bytes per item
```

**How to estimate bytes per item:**
```
tweet_id        = 8 bytes   (64-bit integer)
user_id         = 8 bytes
text (280 chars)= 300 bytes (1 byte per char, round up)
timestamp       = 8 bytes
likes_count     = 4 bytes
retweet_count   = 4 bytes
─────────────────────────
Total           = 332 bytes → round to 500 bytes
```

> Always round up by 20–30% for fields you forgot.

**Architecture signal:**
- < 1 GB/day → single DB fine
- 1 GB–1 TB/day → plan sharding, use S3 for media
- > 1 TB/day → distributed storage mandatory

---

### 5. Storage Over N Years — Long-term capacity

**What it tells you:** Whether you need distributed storage (HDFS, S3), archive strategy

```
Storage (N years) = storage/day × 365 × N
```

**Example:**
```
500 GB/day × 365 × 5 = 912,500 GB ≈ 900 TB over 5 years
```

> Always plan for 5 years in interviews unless told otherwise.

---

### 6. Bandwidth Ingress — Data flowing IN per second

**What it tells you:** Network card capacity, upload rate limits, ingestion pipeline size

```
Ingress bandwidth = Write QPS × bytes per write
```

**Example:**
```
Text: 50 write QPS × 500 bytes = 25,000 bytes/sec = 25 KB/sec
Media: 5 image uploads/sec × 1 MB = 5 MB/sec ingress
```

---

### 7. Bandwidth Egress — Data flowing OUT per second

**What it tells you:** CDN necessity, server count needed, monthly bandwidth cost

```
Egress bandwidth = Read QPS × bytes per response
```

**Example:**
```
10,000 read QPS
Each feed returns 20 tweets × 500 bytes = 10,000 bytes per response

10,000 × 10,000 bytes = 100,000,000 bytes/sec = 100 MB/sec egress
```

**Architecture signal:**
- < 1 MB/sec → single server handles egress
- > 10 MB/sec → CDN strongly recommended
- > 100 MB/sec → CDN mandatory, multi-region

---

### 8. Cache Size — RAM needed for Redis/Memcached

**What it tells you:** Redis instance size, whether you need a cache cluster, eviction policy

```
Cache size = number of hot items × bytes per item

Hot items = apply Pareto principle: 20% of items get 80% of reads
```

**Example:**
```
10,000 read QPS hitting 1M unique items
Hot 20% = 200,000 items
Each item = 500 bytes

Cache size = 200,000 × 500 = 100,000,000 bytes = 100 MB
```

> 100 MB fits comfortably in a single Redis instance. If cache size exceeds ~10 GB, consider Redis Cluster.

---

## 3. Bonus Calculations (Senior Rounds)

### 9. Latency Budget

Break your SLA into component budgets:

```
Total SLA: 100ms

Network (client → server): 20ms
Load balancer:               2ms
App server processing:      15ms
Cache lookup (Redis):        5ms
DB query:                   10ms
Network (server → client):  20ms
Buffer:                     28ms
─────────────────────────────────
Total:                     100ms
```

**Why it matters:** Forces you to justify every architectural layer. If DB needs 50ms you can't fit it in a 100ms SLA — you must cache.

---

### 10. Replication Lag Tolerance

```
Question: how stale can a read be?

Payment system:  0ms tolerance → synchronous replication → PostgreSQL
Social feed:     500ms fine    → async replication → MySQL read replicas
Analytics:       1 hour fine   → batch ETL → data warehouse
```

**Why it matters:** Drives your consistency model (strong vs eventual) and DB choice.

---

### 11. Server Count

```
Server count = Peak QPS ÷ QPS per single server

Typical QPS per server:
  API server (CPU-bound):    5K–10K QPS
  API server (I/O-bound):   20K–50K QPS
  Static file server:       100K+ QPS
```

**Example:**
```
30,000 peak read QPS ÷ 10,000 QPS per server = 3 servers minimum
Add 2x headroom for failures = 6 servers
```

---

### 12. DB Shard Count

```
Shard count = total storage ÷ max storage per shard

Typical max per shard: 500 GB–2 TB (beyond this, queries slow down)
```

**Example:**
```
900 TB over 5 years ÷ 1 TB per shard = 900 shards

→ This tells you: you need consistent hashing and a distributed DB from day one
```

---

## 4. What to Calculate Per System Type

| System | Examples | Must Calculate | Key Insight |
|--------|----------|----------------|-------------|
| **Social feed** | Twitter, Instagram | Write QPS, Read QPS, Ratio, Storage/day, Egress | 200:1 read:write ratio → cache everything |
| **Messaging** | WhatsApp, Slack | Messages/sec, Storage/day, Latency budget | Strict delivery latency SLA → WebSockets, not polling |
| **Video platform** | YouTube, Netflix | Upload BW, Storage/day, Egress BW, CDN | Media dominates — storage + egress cost > compute |
| **Ride sharing** | Uber, Lyft | Location updates/sec, Geo query QPS | 1M location writes/sec → time-series DB, not SQL |
| **Ticketing** | BookMyShow, Ticketmaster | Peak QPS at drop, Concurrency, Cache size | Spiky traffic (1M users in 10s) → virtual queue |
| **URL shortener** | Bitly, TinyURL | Write QPS, Read QPS, Ratio, 5yr Storage | 100:1 read-heavy → classic cache problem |
| **Search** | Google, Elasticsearch | Index size, Query QPS, Latency budget | Index size drives sharding strategy |
| **Payment system** | Stripe, UPI | TPS, Replication lag, 5yr Storage | Zero lag tolerance → synchronous replication |
| **File storage** | Dropbox, Drive | Upload BW, Storage/day, Dedup ratio | Deduplication can cut storage 30–70% |
| **Notifications** | Push, Email, SMS | Events/sec, Fan-out multiplier | Fan-out: 1 event → millions of pushes |

---

## 5. The Universal Framework

Use this sequence for every single system design BOTE question:

```
Step 1 — USERS
  Total users → DAU (assume 10–20% of total are daily active)

Step 2 — ACTIONS
  How many times does each DAU do the core action per day?
  Split reads vs writes separately.

Step 3 — SIZE
  How many bytes is one instance of that action?
  List each field, estimate bytes, sum up, round up 20%.

Step 4 — MULTIPLY
  Writes/day  = DAU × write actions per user
  Reads/day   = DAU × read actions per user
  Storage/day = Writes/day × bytes per write

Step 5 — CONVERT TO PER SECOND
  ÷ 100,000 to get per second (QPS, BW)

Step 6 — SCALE OUT
  × 365 × 5 for 5-year storage
  × 3 for peak traffic headroom

Step 7 — CONCLUDE
  What does each number tell you about the architecture?
  Never leave a number floating — always connect it to a decision.
```

---

## 6. Case Study — Twitter

**System:** Users post tweets, read feeds, follow other users.

### Step 1 — Users
```
Registered users:  500 Million
DAU (20%):         100 Million
```

### Step 2 — Writes (Tweets Posted)
```
5% of DAU post per day = 5 Million users
Each posts 1 tweet/day

Writes/day = 5,000,000
Write QPS  = 5,000,000 ÷ 100,000 = 50 QPS
```

### Step 3 — Reads (Feed Views)
```
Each DAU refreshes feed 10x/day

Reads/day = 100M × 10 = 1 Billion
Read QPS  = 1,000,000,000 ÷ 100,000 = 10,000 QPS
```

### Step 4 — Read:Write Ratio
```
10,000 ÷ 50 = 200:1 (heavily read-dominant)
```

### Step 5 — Storage Per Item
```
tweet_id       = 8 bytes
user_id        = 8 bytes
text           = 300 bytes
timestamp      = 8 bytes
likes_count    = 4 bytes
retweet_count  = 4 bytes
─────────────────────────
Total          = 332 bytes → round to 500 bytes
```

### Step 6 — Storage Per Day
```
Text:  5M tweets × 500 bytes     = 2.5 GB/day
Media: 500K images × 1 MB        = 500 GB/day
Total:                           ≈ 500 GB/day  (media dominates)
```

### Step 7 — Storage Over 5 Years
```
500 GB/day × 365 × 5 = 912,500 GB ≈ 900 TB
```

### Step 8 — Bandwidth
```
Ingress (text):  50 QPS × 500 bytes     = 25 KB/sec
Ingress (media): 5 images/sec × 1 MB    = 5 MB/sec

Egress: 10,000 QPS × (20 tweets × 500 bytes)
      = 10,000 × 10,000 = 100 MB/sec
```

### Architecture Decisions From Numbers

| Calculation | Result | Decision |
|-------------|--------|----------|
| Write QPS | 50/sec | Single DB handles writes fine |
| Read QPS | 10,000/sec | Redis cache mandatory |
| Read:Write | 200:1 | Pre-compute and cache feeds |
| Storage/day | 500 GB | S3 for media, store only URL in DB |
| Egress | 100 MB/sec | CDN mandatory |
| 5yr storage | 900 TB | Distributed storage from day one |

---

## 7. Case Study — WhatsApp

**System:** Users send messages 1:1 and in groups.

### Step 1 — Users
```
Registered users:  2 Billion
DAU (50%):         1 Billion
```

### Step 2 — Messages Per Day
```
Each DAU sends 20 messages/day

Messages/day = 1B × 20 = 20 Billion
Write QPS    = 20,000,000,000 ÷ 100,000 = 200,000 QPS (200K/sec)
```

### Step 3 — Size Per Message
```
message_id   = 8 bytes
sender_id    = 8 bytes
receiver_id  = 8 bytes
content      = 100 bytes (avg text message)
timestamp    = 8 bytes
status       = 1 byte (sent/delivered/read)
──────────────────────────────────────────
Total        = 133 bytes → round to 200 bytes
```

### Step 4 — Storage Per Day
```
Text: 20B messages × 200 bytes = 4,000,000,000,000 bytes = 4 TB/day

Media (20% of messages have media):
  4B media messages × 500 KB avg = 2,000 TB = 2 PB/day

Total: ≈ 2 PB/day
```

### Step 5 — Latency Budget
```
WhatsApp SLA: message delivered in < 500ms

Client → server:  50ms (network)
Server processing: 5ms
DB write:          10ms
Push notification: 50ms
Server → client:  50ms
─────────────────────
Total:            165ms (well within 500ms)
```

### Architecture Decisions From Numbers

| Calculation | Result | Decision |
|-------------|--------|----------|
| Write QPS | 200K/sec | Cassandra (handles 50K QPS per node × 4 nodes) |
| Storage/day | ~2 PB | S3/distributed object store for media |
| Latency budget | 165ms budget | WebSockets for persistent connections |
| Read pattern | Pull on open | Fan-out on read, not write |

---

## 8. Case Study — YouTube

**System:** Users upload and watch videos.

### Step 1 — Users
```
Total users:   2 Billion
DAU (50%):     1 Billion
Uploaders:     1% of DAU = 10 Million uploaders/day
```

### Step 2 — Upload Rate
```
Public figure: 500 hours of video uploaded per minute (real stat)

Per second: 500 × 60 minutes ÷ 60 seconds = 500 hours/min
         = 8.3 hours of video per second
```

### Step 3 — Storage Per Hour of Video
```
Raw 1080p video:       1 GB per hour
YouTube encodes to multiple resolutions:
  360p, 480p, 720p, 1080p, 4K = ~4x multiplier

Stored per hour:       1 GB × 4 = 4 GB
```

### Step 4 — Storage Per Day
```
8.3 hours/sec × 3,600 sec/hour = 29,880 hours/hour
                                ≈ 30,000 hours of video per hour
30,000 hours × 4 GB            = 120,000 GB/hour = 120 TB/hour

Per day: 120 TB × 24            = 2,880 TB/day ≈ 3 PB/day
```

### Step 5 — Storage Over 5 Years
```
3 PB/day × 365 × 5 = 5,475 PB ≈ 5.5 Exabytes
```

### Step 6 — Egress Bandwidth
```
1B DAU × watch 30 min/day = 30B minutes/day = 500M minutes/sec

Wait, convert properly:
30B minutes/day ÷ 1,440 min/day = 20.8M concurrent viewers

Each stream at 720p = 1 Mbps (1 Megabit = 125 KB/sec)

Egress = 20.8M × 1 Mbps = 20.8 Tbps (terabits per second)
```

### Architecture Decisions From Numbers

| Calculation | Result | Decision |
|-------------|--------|----------|
| Storage/day | 3 PB | Distributed storage (GCS/S3), never local disk |
| 5yr storage | 5.5 EB | Tiered storage — hot/warm/cold archiving |
| Egress | 20 Tbps | Massive CDN (YouTube has 100s of PoPs globally) |
| Upload rate | 500 hrs/min | Async transcoding pipeline (message queue) |

---

## 9. Case Study — Uber

**System:** Drivers send GPS location every few seconds, riders request rides.

### Step 1 — Users
```
Active drivers globally:  5 Million
Active riders globally:   10 Million (at peak)
```

### Step 2 — Location Updates Per Second
```
Each driver sends location every 5 seconds

Updates/sec = 5,000,000 ÷ 5 = 1,000,000 = 1M location updates/sec
```

### Step 3 — Size Per Location Update
```
driver_id    = 8 bytes
latitude     = 8 bytes (double)
longitude    = 8 bytes (double)
timestamp    = 8 bytes
bearing      = 4 bytes (direction of travel)
speed        = 4 bytes
─────────────────────────────────
Total        = 40 bytes → round to 50 bytes
```

### Step 4 — Storage Per Day (Location Data)
```
1M updates/sec × 50 bytes = 50 MB/sec

Per day: 50 MB/sec × 100,000 sec = 5,000,000 MB = 5 TB/day
```

### Step 5 — Geo Query QPS (Rider Looking for Driver)
```
10M active riders, each polls for nearby drivers every 5 sec

Geo queries/sec = 10,000,000 ÷ 5 = 2,000,000 = 2M geo queries/sec
```

### Architecture Decisions From Numbers

| Calculation | Result | Decision |
|-------------|--------|----------|
| Write QPS | 1M/sec | Cassandra or DynamoDB — SQL can't handle this |
| Storage/day | 5 TB | Time-series DB (InfluxDB), not relational |
| Geo queries | 2M/sec | Geospatial index (PostGIS, Redis GEO, S2 geometry) |
| Data freshness | 5 sec | Push model from driver → server, not polling |

---

## 10. Case Study — URL Shortener

**System:** Convert long URLs to short codes, redirect when visited.

### Step 1 — Users
```
Registered users:   100 Million
DAU (10%):          10 Million
```

### Step 2 — Write QPS (URLs Created)
```
1% of DAU create a short URL/day = 100,000 URLs/day

Write QPS = 100,000 ÷ 100,000 = 1 QPS
```

### Step 3 — Read QPS (Redirects)
```
Each short URL gets clicked 10x/day on average

Reads/day = 100,000 × 10 = 1,000,000
Read QPS  = 1,000,000 ÷ 100,000 = 10 QPS
```

### Step 4 — Read:Write Ratio
```
10 ÷ 1 = 10:1
```

### Step 5 — Storage Per URL
```
short_code   = 7 bytes   (e.g. "abc1234")
long_url     = 512 bytes (average URL length)
user_id      = 8 bytes
created_at   = 8 bytes
expiry       = 8 bytes
click_count  = 8 bytes
──────────────────────────────
Total        = 551 bytes → round to 600 bytes
```

### Step 6 — Storage Per Day
```
100,000 new URLs/day × 600 bytes = 60,000,000 bytes = 60 MB/day
```

### Step 7 — Storage Over 5 Years
```
60 MB/day × 365 × 5 = 109,500 MB ≈ 110 GB over 5 years
```

### Step 8 — Short Code Length
```
Base62 encoding (a-z, A-Z, 0-9 = 62 characters)

7 characters: 62^7 = 3.5 Trillion unique URLs
100,000 URLs/day × 365 × 5 years = 182 Million URLs

3.5 Trillion >> 182 Million → 7 chars is more than enough
```

### Architecture Decisions From Numbers

| Calculation | Result | Decision |
|-------------|--------|----------|
| Write QPS | 1/sec | Any single DB handles this trivially |
| Read QPS | 10/sec | Still trivial, but cache top URLs in Redis |
| 5yr storage | 110 GB | Single DB, no sharding needed |
| Redirect latency | Must be <10ms | Redis cache for hot URLs, DB miss for cold |

---

## 11. Case Study — BookMyShow

**System:** Users browse events and book seats. Handle flash sales.

### Step 1 — Users
```
Registered users:  50 Million
DAU (10%):         5 Million
Peak event (Coldplay): 1 Million concurrent users in 10 seconds
```

### Step 2 — Normal Traffic QPS
```
5M DAU × 5 page views/day = 25M views/day
Read QPS (normal) = 25,000,000 ÷ 100,000 = 250 QPS

Booking conversion = 1% of DAU
Write QPS (bookings) = 50,000 ÷ 100,000 = 0.5 QPS
```

### Step 3 — Peak Traffic (Flash Sale)
```
1 Million users, all click buy in 10 seconds

Peak QPS = 1,000,000 ÷ 10 = 100,000 QPS (100K/sec)

This is 400x normal traffic → normal architecture collapses
```

### Step 4 — Seat Inventory Size
```
Typical venue:  50,000 seats
Seat object:
  seat_id      = 8 bytes
  row          = 2 bytes
  number       = 4 bytes
  status       = 1 byte
  price        = 8 bytes
  version      = 4 bytes  (for optimistic locking)
  ──────────────────────
  Total        = 27 bytes → round to 50 bytes

Full inventory = 50,000 × 50 bytes = 2.5 MB
```

### Step 5 — Cache Calculation
```
Full seat inventory = 2.5 MB
Fits entirely in Redis with room to spare.

At 100K QPS, serving from Redis = 100,000 × 2.5 MB??

No — you serve per-seat lookups, not full inventory:
100K QPS × 50 bytes per seat = 5 MB/sec read bandwidth
→ Redis handles this trivially (100K+ QPS capable)
```

### Architecture Decisions From Numbers

| Calculation | Result | Decision |
|-------------|--------|----------|
| Normal QPS | 250/sec | Single DB fine for browsing |
| Peak QPS | 100K/sec | Virtual waiting room before DB |
| Inventory size | 2.5 MB | Entire inventory fits in Redis |
| Concurrency | 1M users, 50K seats | 95% get failure → optimize for graceful rejection |

---

## 12. Numbers Worth Memorizing

### Real Platform Stats

| Platform | DAU | Key stat |
|----------|-----|----------|
| WhatsApp | 1 Billion | 100B messages/day |
| Facebook | 2 Billion | 500K photos/min uploaded |
| Instagram | 500 Million | 100M photos/day |
| Twitter/X | 250 Million | 500M tweets/day |
| YouTube | 2 Billion users | 500 hrs video uploaded/min |
| Uber | 5M active drivers | 1M location updates/sec |

### Typical Object Sizes

| Object | Size |
|--------|------|
| UUID (16 bytes stored as binary) | 16 bytes |
| UUID (stored as string) | 36 bytes |
| Unix timestamp | 4–8 bytes |
| Integer (int32) | 4 bytes |
| Integer (int64) | 8 bytes |
| Boolean | 1 byte |
| Tweet (text only) | ~300 bytes |
| Average DB row | 500 bytes–2 KB |
| Average JSON API response | 1–5 KB |
| Compressed photo | 1 MB |
| 1 min video (720p, compressed) | 10 MB |
| 1 hour video (1080p, compressed) | 1 GB |
| 1 hour video (all resolutions) | 4 GB |

### DB QPS Capacity (Rough Estimates)

| Database | Max QPS (single node) | Use case |
|----------|-----------------------|----------|
| MySQL / PostgreSQL | 5K–10K | Transactional, relational |
| Redis | 100K–1M | Cache, sessions, locks |
| Cassandra (per node) | 50K | Write-heavy, time-series |
| MongoDB | 10K–30K | Document store |
| Elasticsearch | 1K–5K | Full-text search |

### Network Speeds

| Connection | Bandwidth |
|------------|-----------|
| 4G mobile | 10–50 Mbps |
| Home broadband | 100–1000 Mbps |
| Data center NIC | 10–100 Gbps |
| Cross-region latency (US ↔ India) | ~160ms RTT |
| Cross-region latency (US ↔ Europe) | ~80ms RTT |
| Same data center | < 1ms |

---

## 13. Common Mistakes

### 1. Leaving numbers floating
❌ "We'll have 10,000 read QPS."
✅ "We'll have 10,000 read QPS — this exceeds what a single MySQL instance can handle, so we need Redis cache and read replicas."

### 2. Forgetting peak traffic
Always multiply average by 3x for peak headroom:
```
Peak QPS = average QPS × 3
```

### 3. Ignoring media
Media almost always dominates storage. Always ask: "Does this system have images or video?"
```
Twitter text storage:  2.5 GB/day
Twitter media storage: 500 GB/day  ← 200x more
```

### 4. Using wrong DAU assumption
- Consumer apps: 10–20% of registered users are DAU
- Messaging apps: 40–60% (WhatsApp, Slack)
- B2B tools: 5–10%

### 5. Not converting to architecture decisions
Every number must lead somewhere. The sequence is:
```
Number → Bottleneck → Solution
50 write QPS → single DB fine → no special write handling needed
10,000 read QPS → DB can't handle → add Redis cache
100 MB/sec egress → server can't push this → CDN mandatory
```

### 6. Forgetting the 5-year storage question
Interviewers almost always ask this. Pre-calculate it for every case study.

---

## Quick Reference Card

```
TRAFFIC
  Write QPS = (DAU × writes/user) ÷ 100,000
  Read QPS  = (DAU × reads/user)  ÷ 100,000
  Ratio     = Read QPS ÷ Write QPS

STORAGE
  Per day   = writes/day × bytes/item
  Per year  = per day × 365
  5 years   = per day × 365 × 5

BANDWIDTH
  Ingress   = Write QPS × bytes/write
  Egress    = Read QPS × bytes/response

CACHE
  Size      = hot items count × bytes/item
  Hot items = 20% of total (Pareto)

SERVERS
  Count     = Peak QPS ÷ QPS per server × 2 (headroom)

SHARDS
  Count     = total storage ÷ max per shard

TIME
  1 day     = 100,000 seconds (rounded)
  1 year    = 30,000,000 seconds (rounded)
```

---

*Last updated: 2024 | For FAANG system design interview preparation*
