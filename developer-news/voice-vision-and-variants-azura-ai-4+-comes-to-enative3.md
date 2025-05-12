# Voice, Vision, and Variants: Azura AI 4+ Comes to eNative3

Hold onto your keyboards, devs — it’s finally here. The future of voice, chat, and contextual computing just _plugged into_ your favorite framework: **Azura AI 4+ now integrates seamlessly into eNative3** via the shiny new `azai` add-on. You can grab it right now through the CLI or from [downloads.edc.ecowestern.com](https://downloads.edc.ecowestern.com).

Whether you're building a charming little chatbot, a sleek voice-controlled productivity suite, or an emotional support squirrel app (we won't judge), Azura AI 4+ is your new best friend — smart, fast, and _freakishly_ aware of context.

Let’s get into it.

***

## Meet the AZAI Add-on for eNative3

eNative3's event-driven, modular architecture already makes it a joy to build with. Now, with Azura AI 4+ built right into your apps via the `azai` addon, you get access to:

* 💬 **Multi-tone text variants** (conversational, professional, creative, etc.)
* 🧠 **Real-time contextual awareness**
* 🎤 **Multimodal input** (voice & text)
* 🔄 **Session-based memory**
* 🔌 **Effortless event dispatch integration**

### Quick Install

Want to get started fast? From your eNative project root:

```
enative add azai
```

### Or download the bundle manually:

```
wget https://downloads.edc.ecowestern.com/addons/azai.enative
```

Then mount it:

{% code title="enative" %}
```enative
import { useAZAI } from "azai"

const azura = useAZAI("your-api-key-here")
```
{% endcode %}

Boom. You’re talking to Azura.

***

## Let’s Talk Text Variants

Azura AI 4+ ships with **six distinct text styles** you can swap in and out _on the fly_:

* `"conversational"` — casual and friendly
* `"professional"` — formal and polished
* `"creative"` — quirky and expressive
* `"instructional"` — clear and direct
* `"empathetic"` — soft and emotionally aware
* `"neutral"` — just the facts, no frills

Want to build a bot that greets users like a human, explains features like a teacher, and comforts them like a therapist? Go for it.

```enative
enativeCopyEditazura.ask({
  input: "Hey Azura, explain how to deploy this app!",
  text_variant: "instructional",
  context: {
    project: "AzuraChat",
    skill_level: "intermediate"
  }
})
```

***

## Deep Context = Real Conversations

Azura doesn’t just spit out replies — it _remembers_. With session-based memory, it tracks the flow of your user’s input, tone, and intent.

Here’s a lightweight chat handler:

```enative
enativeCopyEditon("user.message", async ({ text, session }) => {
  const response = await azura.ask({
    input: text,
    session_id: session.id,
    contextual_state: {
      previous_input: session.lastPrompt
    }
  })

  emit("bot.response", response.output)
})
```

And just like that, your bot isn’t just reactive — it’s _aware_.

***

## Voice Input? Yep. That Too.

Want voice control baked in? Azura AI 4+ handles audio streams with elegance. Use native microphone events from eNative3's input handler, then pass them through `azura.transcribe()` and respond in real time.

```enative
enativeCopyEditon("mic.audio", async ({ audioBuffer }) => {
  const text = await azura.transcribe(audioBuffer)
  const reply = await azura.ask({ input: text, text_variant: "empathetic" })

  speak(reply.output)
})
```

This is peak sci-fi, and you’re building it with 5 lines of code.

***

## Pro Tip: Build with EADS

Sure, eNative3 plays well with everything. But if you’re serious about AI development in the EcoWestern ecosystem, you owe it to yourself to build in **EADS** (EcoWestern Application Development Studio). Why?

* ⚡️ **One-click deploy to every EcoWestern platform**
* 🧩 **Auto-UI integration for MateOS, MatePC, and more**
* 🔧 **Built-in debug tools for Azura AI**
* 🌐 **Cloud sync and project sharing with your team**

It's everything you already love about eNative, but running on **developer-grade rocket fuel**.

***

## Wrapping Up

Azura AI 4+ was already one of the most powerful AI assistant platforms available to developers. Now with native support in eNative3 — via the `azai` add-on — it’s officially in your hands.

From sleek assistants to powerful voice interfaces and emotionally aware chatbots, your next app just leveled up _hard_.

Download the `azai` add-on today. Start with the CLI or head to [downloads.edc.ecowestern.com](https://downloads.edc.ecowestern.com).

And hey — don’t just build. **Build smarter.**\
💙 with Azura AI and eNative3.
