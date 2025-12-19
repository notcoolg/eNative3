---
description: 🧠 Azura AI 5 for Developers
---

# Using Azura AI 5 with eNative3 via Economic Cloud Services (ECS)

### Using Azura AI 5 with eNative3 via Economic Cloud Services (ECS)

Azura AI 5 isn’t just a smarter assistant, it’s a **developer platform**.

With the launch of **Azura AI 5**, EcoWestern is introducing a structured, scalable way for developers to use AI responsibly through **Economic Cloud Services (ECS)**. Whether you’re building lightweight on-device features or deep reasoning workflows, ECS gives you **clear limits, predictable pricing, and fine-grained control**.

This article explains:

* The Azura AI 5 model lineup (developer view)
* Token pricing per model
* ECS tiers and what you get
* How to call Azura AI 5 from eNative3
* Why ECS + EADS is the best way to build

***

### 🧩 Azura AI 5 Models (Developer Breakdown)

Azura AI 5 models are **composable**, you don’t pick one and lock in. You route requests to the _right_ model based on task complexity.

| Model               | Designed For              | Typical Use                         |
| ------------------- | ------------------------- | ----------------------------------- |
| **5 Nano**          | Ultra-fast, on-device     | Commands, short answers, UI actions |
| **5 Basic**         | Longer responses          | Help text, summaries                |
| **5 Plus**          | High context + multimodal | Productivity, creative, images      |
| **5 Consideration** | Deep reasoning            | Research, planning, analysis        |

***

### 💰 Token Pricing (Azura AI 5)

Azura AI uses **token-based pricing**, billed against your ECS monthly credits or usage pool.

> Tokens include **input + output combined**

#### 📊 Token Costs (USD)

| Model                        | Cost per 1M Tokens |
| ---------------------------- | ------------------ |
| **Azura AI 5 Nano**          | $0.15              |
| **Azura AI 5 Basic**         | $0.40              |
| **Azura AI 5 Plus**          | $1.20              |
| **Azura AI 5 Consideration** | $3.00              |

✔ On-device Nano usage does **not** consume ECS tokens\
✔ Tool calls (search, multimodal processing) are billed separately but clearly surfaced\
✔ You always see the estimated cost _before_ a request is escalated

***

### ☁️ Economic Cloud Services (ECS) – AI Access Plans

Azura AI 5 is accessed through **Economic Cloud Services**, which also powers storage, identity, analytics, and more.

> These tiers include **many ECS services beyond AI**\
> Learn more: **economiccloud.com/service-packs**

***

#### 🆓 Free Tier (Credit Card Required)

**$0 / month**

* Access to: **Nano, Basic, Plus**
* **30 API calls / second**
* **100M calls / month**
* Ideal for testing, prototypes, and small apps

***

#### ⭐ Classic Tier

**$8.99 / month**

* Access to: **All models (including Consideration)**
* **100 calls / second**
* **300M calls / month**
* Best for indie developers and small teams

***

#### 🚀 Super Tier

**$18.99 / month**

* Everything in Classic
* **300 calls / second**
* **Unlimited calls per month**
* Great for production apps at scale

***

#### 🧠 Ultra Tier

**$29.99 / month**

* Everything in Super
* **1000 calls / second**
* **$15 free credit every month**
* Designed for high-traffic or AI-first apps

***

#### 🧑‍💻 EDC Exclusive: AI Service Pack

**$13.99 / month** _(EDC members only)_

* **600 calls / second**
* **500M calls / month**
* **$8 AI credit included**
* Full access to all Azura AI 5 models
* Tuned specifically for AI-heavy workloads

> This is the **best value plan** if you’re actively building with Azura.

***

### 🔐 Authentication & Setup

All Azura AI 5 API usage requires:

* An **ECS project**
* An **API key**
* Optional per-app or per-user quotas

```enative
import { useAZAI } from "azai"

const azura = useAZAI({
    apiKey: ECS.env("AZURA_API_KEY"),
    project: "com.myapp.azura"
})
```

***

### 💬 Making Your First Request

#### Simple (Nano / Basic)

```enative
const response = await azura.ask({
    input: "What does this setting do?",
    model: "azura-5-basic"
})

print(response.output)
```

***

#### Contextual + Plus Model

```enative
await azura.ask({
    input: "Summarize this document",
    model: "azura-5-plus",
    context: {
        docType: "legal",
        tone: "professional"
    }
})
```

***

#### Deep Reasoning (Consideration)

```enative
await azura.ask({
    input: "Compare these two strategies and recommend one",
    model: "azura-5-consideration",
    allowEscalation: true
})
```

Before execution, ECS will surface:

* Estimated token usage
* Cost
* Tool calls involved

***

### 🔄 Smart Escalation (Recommended Pattern)

Azura AI 5 is designed to **escalate intelligently**.

```enative
await azura.ask({
    input: userInput,
    model: "azura-5-nano",
    escalation: {
        to: "azura-5-plus",
        when: "confidence < 0.6"
    }
})
```

This keeps:

* Costs low
* Latency minimal
* Answers accurate

***

### 🛠️ Why You Should Build This in EADS

Yes, you _can_ build Azura apps anywhere.

But **EADS** gives you:

* Live token & cost tracking
* AI request simulation
* Model-switch visualization
* ECS quota warnings before you hit limits
* One-click deploy across EcoWestern platforms

If you’re serious about Azura AI development, **EADS is the reference environment**.

***

### 🧭 Final Notes

Azura AI 5 + ECS is designed to be:

* Transparent
* Predictable
* Scalable
* Developer-first

You always know:

* Which model you’re using
* What it costs
* Why it was chosen

That’s not just better AI, it’s **better engineering**.
