<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:6366f1,50:38bdf8,100:34d399&height=180&section=header&text=Akhilesh+Singh&fontSize=40&fontColor=ffffff&animation=fadeIn&fontAlignY=36&desc=Backend+Engineer+%E2%80%94+Real-time+Systems+%C2%B7+Multi-tenant+Architecture&descSize=14&descAlignY=58&descColor=d0d0ff" width="100%"/>

<br/>

[![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&size=15&duration=3000&pause=1000&color=8b8fa8&center=true&vCenter=true&width=620&lines=I+build+systems+that+hold+up+under+real+load.;Not+just+in+demos.;If+it+feels+like+hell%2C+stand+up+and+put+on+music.;Then+come+back+and+fix+it+properly.)](https://git.io/typing-svg)

<br/>

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0d1117?style=for-the-badge&logo=linkedin&logoColor=6366f1&labelColor=0d1117)](https://www.linkedin.com/in/akhilesh-singh-dev)&nbsp;
[![Twitter](https://img.shields.io/badge/Twitter-0d1117?style=for-the-badge&logo=x&logoColor=e8e8f0&labelColor=0d1117)](https://x.com/singh_akhil2272)&nbsp;
[![Email](https://img.shields.io/badge/Email-0d1117?style=for-the-badge&logo=gmail&logoColor=34d399&labelColor=0d1117)](mailto:singhakhilesh19468@gmail.com)

</div>

<br/>

---

## About

When I hit a problem, I don't context-switch — I go in. Full focus until it breaks or I do. If it's the kind of problem where nothing makes sense and the logs are lying to you, I stand up, put on music, walk it off, and come back with a clearer head. Usually that's when I realize the bug was two layers above where I was looking.

I care about the decisions that don't show up in tutorials — the edge cases, the failure modes, the tradeoffs that only matter when real users show up. The stuff that seems fine until it very suddenly isn't.

A few things I actually believe:

- **Don't touch working dependencies.** Updating a library because a newer version exists is how you spend a Friday debugging something that had nothing to do with your feature. Latest ≠ better. Stable = underrated.
- **Flat is better than nested.** If I have to open 20 files to understand one thing, the architecture has failed before the code did. Route → Controller → Service → Repository. Every layer earns its place.
- **Failure handling isn't an afterthought.** If your async job fails silently and nobody retries it, you don't have a system — you have a demo that works on good days.

---

## Projects

### [FlowSpace](https://github.com/Akhilesh-Singh-0/flowspace) &nbsp;·&nbsp; Real-time Project Management Backend

![Node.js](https://img.shields.io/badge/Node.js-0d1117?style=flat-square&logo=node.js&logoColor=34d399)&nbsp;
![TypeScript](https://img.shields.io/badge/TypeScript-0d1117?style=flat-square&logo=typescript&logoColor=38bdf8)&nbsp;
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-0d1117?style=flat-square&logo=postgresql&logoColor=6366f1)&nbsp;
![Redis](https://img.shields.io/badge/Redis-0d1117?style=flat-square&logo=redis&logoColor=ef4444)&nbsp;
![BullMQ](https://img.shields.io/badge/BullMQ-0d1117?style=flat-square&logo=bull&logoColor=f59e0b)&nbsp;
![Docker](https://img.shields.io/badge/Docker-0d1117?style=flat-square&logo=docker&logoColor=38bdf8)

Multi-tenant project management backend. The interesting part isn't the tasks and workspaces — it's what happens when two clients are looking at the same workspace and one of them makes a change.

```
Client  →  REST API  →  Redis Pub/Sub  →  WebSocket Server  →  Clients
```

The API and WebSocket server are decoupled deliberately. Redis sits in the middle as the message broker — so a crash in one doesn't cascade into the other, and you can scale them independently without a rewrite.

- **RBAC with real constraints** — last-owner protection, role scoping per workspace, permission checks at the service layer not just a middleware check
- **WebSocket lifecycle** — heartbeat-based health tracking, automatic cleanup on stale connections, no silent memory leaks
- **BullMQ for async work** — side effects run in the background, failures retry with exponential backoff, the request path stays clean
- **Horizontally scalable** — new API instances can join without coordination because Redis handles the messaging

---

### [Luminary](https://github.com/Akhilesh-Singh-0/luminary) &nbsp;·&nbsp; Second Brain for the Web

![React](https://img.shields.io/badge/React_18-0d1117?style=flat-square&logo=react&logoColor=38bdf8)&nbsp;
![TypeScript](https://img.shields.io/badge/TypeScript-0d1117?style=flat-square&logo=typescript&logoColor=6366f1)&nbsp;
![MongoDB](https://img.shields.io/badge/MongoDB-0d1117?style=flat-square&logo=mongodb&logoColor=34d399)&nbsp;
![Express](https://img.shields.io/badge/Express-0d1117?style=flat-square&logo=express&logoColor=ffffff)&nbsp;
![Vercel](https://img.shields.io/badge/Vercel-0d1117?style=flat-square&logo=vercel&logoColor=ffffff)

Save, organize, and share content from across the web. The CRUD was done in an afternoon. The share system took considerably longer — because the obvious solution leaks user identity and the obvious solution is usually wrong.

The requirement: one public URL for your entire collection, with zero trace of who you are. Not "hard to guess" — actually anonymous.

`nanoid` hash → maps to user server-side only. The URL carries nothing. No user ID, no sequential patterns, nothing that tells you anything about the person behind it. Persistent, shareable, and completely opaque by design. That decision restructured the entire sharing layer — which is exactly when you know it was the right call.

---

## Things I've Debugged So You Don't Have To

| The problem | What actually fixed it |
|---|---|
| **WebSocket connections not closing** | Missing disconnect handlers → silent memory leak. Fixed with a heartbeat that detects dead connections and cleans them up properly |
| **"Why is this library broken"** | It wasn't. Updated a dependency mid-project, new version had breaking changes, spent two hours on something unrelated to the feature. Pinned everything after that |
| **RBAC that breaks at the edges** | `if role === 'admin'` isn't RBAC, it's a vibes check. Real constraints live at the service layer: last-owner protection, role-change authorization, workspace-scoped permissions |
| **Async failures no one notices** | If your job queue silently drops failures, your system is lying to you. BullMQ with retry + backoff means failures are visible, recoverable, and isolated from the request path |
| **Folders inside folders inside folders** | Flat architecture. Route → Controller → Service → Repository. If you need a map to find the code, the structure lost before the feature shipped |

---

## How I Think About Systems

Before writing code, I ask:

> *What breaks at 10× load?*
>
> *Which failures are acceptable, and which ones cascade quietly?*
>
> *Where does shared state live, and who actually owns it?*
>
> *What does this design accidentally reveal?*

The first three shaped FlowSpace. The last one shaped Luminary's sharing layer. Good questions are worth more than fast answers.

---

## Stack

![Node.js](https://img.shields.io/badge/Node.js-0d1117?style=for-the-badge&logo=node.js&logoColor=34d399)&nbsp;
![TypeScript](https://img.shields.io/badge/TypeScript-0d1117?style=for-the-badge&logo=typescript&logoColor=38bdf8)&nbsp;
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-0d1117?style=for-the-badge&logo=postgresql&logoColor=6366f1)&nbsp;
![MongoDB](https://img.shields.io/badge/MongoDB-0d1117?style=for-the-badge&logo=mongodb&logoColor=34d399)&nbsp;
![Redis](https://img.shields.io/badge/Redis-0d1117?style=for-the-badge&logo=redis&logoColor=ef4444)&nbsp;
![React](https://img.shields.io/badge/React-0d1117?style=for-the-badge&logo=react&logoColor=38bdf8)&nbsp;
![Docker](https://img.shields.io/badge/Docker-0d1117?style=for-the-badge&logo=docker&logoColor=38bdf8)&nbsp;
![Git](https://img.shields.io/badge/Git-0d1117?style=for-the-badge&logo=git&logoColor=f59e0b)

---

## Currently

Going deeper into distributed systems — not the "I've used Redis" kind of deeper, but the actual consistency models, partition tolerance tradeoffs, and why databases sometimes lie to you and call it a feature.

Open to backend roles. If your system has real failure modes and you care about getting the architecture right rather than just getting it shipped, let's talk.

<br/>

<div align="center">
<img src="https://capsule-render.vercel.app/api?type=waving&color=0:34d399,50:38bdf8,100:6366f1&height=100&section=footer" width="100%"/>
</div>
