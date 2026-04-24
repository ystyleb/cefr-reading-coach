---
tags:
- english-reading
date: 2026-04-24
source_url: https://atproto.com/blog/serving-the-for-you-feed
cefr_level: B1.2
lexile: 700-900L
direction: AI / tech
comprehension_score: 82
---

## Original Title

Serving the For You feed
(Source: spacecowboy guest post on atproto.com, surfaced by Simon Willison)

## Rewritten Version

### One Gaming PC, 72,000 Users, $30 a Month

A Bluesky user who calls himself **spacecowboy** runs the **For You** feed. About 72,000 people open this feed every day. Most readers would guess that such a service must live in a large data center. In fact, it lives in his living room.

The whole feed runs on a single gaming PC plugged into his TV. The machine has a 16-core chip, 96GB of memory, and two fast hard drives. It is also connected to a backup battery, so a short **outage** will not bring the service down.

The idea behind the feed is simple. It looks at the posts you have liked. Then it finds other people who liked the same posts. Finally, it shows you the other posts those people liked recently. All of this data is stored in a single SQLite database, which is now 419GB in size. The setup is small but easy to manage.

His home PC cannot be reached from the internet directly. So spacecowboy rents a tiny server for $7 a month, sitting in a real data center. That tiny server takes the public traffic and **forwards** every request to his home PC over a private network. Together, the two parts handle 15 to 25 requests per second. The feed has had **99.8% uptime** since last year.

Total monthly cost: around **$30**. That includes $20 for electricity, $7 for the rented server, and $3 for two domain names. A similar cloud server would cost about $245 a month.

He also **estimates** that the same setup could already serve 30 times more users. That is close to every active person on Bluesky today — all from one gaming PC, which still plays games at night.

## Vocabulary

- **outage** /ˈaʊtɪdʒ/ [n] a period when a service or power stops working — *"a short outage will not bring the service down"*
  ![[outage.mp3]]
- **forward** /ˈfɔːrwərd/ [v] to send a request on to another machine or person — *"the tiny server forwards every request to his home PC"*
  ![[forward.mp3]]
- **uptime** /ˈʌptaɪm/ [n] the percentage of time a system stays online and working — *"The feed has had 99.8% uptime since last year"*
  ![[uptime.mp3]]
- **estimate** /ˈestɪmeɪt/ [v] to make a rough calculation or guess — *"He estimates that the same setup could serve 30 times more users"*
  ![[estimate.mp3]]

## My Summary (Native Language — Chinese)

> 一个 Blue Sky 的用户叫 Space Cowboy，他在运营 For You Feed，大概有 72000 名用户每天都会看。大部分读者都会猜这个服务跑在一个很大的数据中心里面，但事实上他跑在 Space Cowboy 的起居室里。整个信息流跑在一个游戏 PC 上，连接着他的电视。这个机器有 16 个核心，96GB 内存，还有两个非常快的物理硬盘，它还连着一个备用的电池。所以如果是短暂的断电的话，也不会使这个服务停机。
>
> 这个信息流的想法很简单：他会看你喜欢的帖子，然后找到那些同样点了 like 的人，最后给你展示这些人最近还点了 like 的那些帖子。所有数据都存储在一个 SQLite 的数据库里边，这是个单节点的数据库，现在已经 419GB 大。"步伐很小，但是容易去管理"——这句表面直译明白，但具体在说什么不太清楚。
>
> 他家里的 PC 不能直接连接外部因特网，所以 Space Cowboy 每月花 7 美金租了一个小服务器在一个真实数据中心。这个小服务器会把公众请求转发到他家里的 PC 上，通过私有网络。两部分每秒处理 15~25 次请求。在线时间 99.8%。每月花费大概 30 美金（20 电力 + 7 租设备 + 3 两个域名）。一个小云端服务可能要 240 多一个月。
>
> 他也做了同样的事，在云端，现在差不多服务了 30 倍的用户——这句刚开始没看明白。这基本上是 Blue Sky 上所有用户每天的产生的行为了。这些都是从一个游戏 PC 完成的，而且每天晚上他还在这个 PC 上玩游戏。

## Feedback

### Content layer

**✅ Caught**:
- Algorithm logic with nested relative clauses (who liked the same posts → the other posts they liked) — fully parsed; Day 01 subject-inversion problem is now stable
- All numbers exact: 72000 users / 16 cores / 96GB / 419GB / 99.8% uptime / $7 / $20 / $3 / $30 / 15-25 QPS
- Core dramatic contrast (data center vs living room; $30 vs $245)

**❌ Missed / misread**:
- *"He also estimates that the same setup could already serve 30 times more users."* read as "he's already done the same thing in the cloud, serving 30x users".
  - Actual meaning: he **estimates** this same setup (no upgrade needed) **already has the capacity** to serve 30x more users — this is **theoretical headroom**, not actual usage.
  - Grammar lesson: `could already serve` — `could` here is potential-capacity virtual modality, not past tense; `already` modifies the *capacity* the system already has, not an action that has happened.
  - Why this matters: this is the article's punchline. 72000 × 30 ≈ 2.1M ≈ all active Bluesky users — meaning one gaming PC could theoretically serve the entire platform.

**❓ Self-flagged confusion**:
- *"The setup is small but easy to manage"* — translated literally fine but unclear what "setup" refers to. Setup = the entire architecture. Implied contrast: a traditional service for this many users would need microservices + Redis + Kafka, while his is one Go process + one SQLite file.
- *"30 times more users"* — see above.

**Minor**: $245 misheard as "240 point 5" — English "two hundred forty-five" has no decimal; mind the hundreds magnitude.

**Score: 82/100**
- 11/12 core facts caught
- 1 substantive misread of the central argument
- Self-flagged 2 confusion points (metacognition bonus)

### Language layer

Native-language (Chinese) summary — language correction skipped per skill rules.

### Next session suggestion

- Level: **hold at B1.2**
- Reason: Consecutive pass #2 (Day 02: 85, Day 03: 82). Rule requires 3 consecutive ≥ 80% to step up to B2.1. Today's 82 < 85 with one core-argument misread; one more stable ≥ 80% will confirm B1.2 is digested rather than barely passing.
- Next session focus: `could + adverb + verb` virtual-modality structures
