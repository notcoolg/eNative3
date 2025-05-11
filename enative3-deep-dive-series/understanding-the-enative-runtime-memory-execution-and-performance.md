# 🔍 Understanding the eNative Runtime: Memory, Execution, and Performance

**Applies to:** eNative3 RS1+\
**Skill Level:** Intermediate to Advanced\
**Last Updated:** May 2025

***

## 📘 Overview

Welcome to the first article in the **eNative3 Deep Dive Series**, where we’ll peel back the layers of EcoWestern’s most powerful programming language. In this issue, we’re diving into the **eNative3 runtime system** — how it works, how it thinks, and how you can write code that works _with_ it rather than _against_ it.

The eNative3 runtime has been fully rewritten from scratch (as of RS0), offering a **3814x efficiency improvement** over eNative2, with better memory orchestration, a new task resolver, and modern instruction sets tailored for **EPU-class hardware**. Let’s unpack what all that means.

***

## ⚙️ Core Runtime Engine

The eNative runtime is a **hardware-aware, event-driven execution engine**. When you compile your code, it doesn’t just become machine instructions — it becomes runtime-managed tasks with layered priorities, bound to system-level hooks and kernel extensions.

Runtime services are categorized as:

| Layer             | Function                                        | Key Service               |
| ----------------- | ----------------------------------------------- | ------------------------- |
| **L0 (Hardware)** | Interfacing directly with EPU (or legacy) chips | Kernel Hooks, Drivers     |
| **L1 (System)**   | Runtime memory, thread, and interrupt mgmt      | CoreScheduler, Allocators |
| **L2 (API)**      | Developer-facing functions and classes          | Standard Library, Modules |
| **L3 (Custom)**   | User-defined code                               | Your App Logic            |

The engine constantly optimizes where and how code runs based on:

* Thread count availability
* Heat/power limitations (via EPU telemetry)
* Memory footprint + GC cycle timing
* Task urgency (e.g., foreground vs background execution)

***

## 🧠 Memory Handling: No More Guesswork

eNative3’s runtime uses **adaptive scoped memory**, meaning:

* Memory is **automatically cleaned up** when it leaves its context.
* You can override default behavior using _Managed Blocks_ or _Fast Zones_.
* No manual `delete` needed unless using legacy pointer overrides.

```cpp
#include <enative/memory.h>

void runScoped() {
    eNative::ScopedAlloc mem;
    int* data = mem.alloc<int>();
    *data = 42; // Automatically freed at end of scope
}
```

Want to override this? Use **Manual Blocks**:

```cpp
int* manual = new int(99);
delete manual; // You manage it now.
```

Use memory maps wisely in high-frequency modules. Overuse can trigger forced GC cycles, which slows execution. Always run **`ewen inspect --mem`** if unsure.

***

## ⛓️ Execution Model: Task-Driven Threads

All code in eNative3 executes inside **Tasks**, which are automatically assigned a priority, duration cap, and resource weight by the runtime. This is why there’s **no `main()`** function anymore for standard apps — your entry point is a declared task module.

```cpp
#include <enative/task.h>

class StartupTask : public eNative::Task {
public:
    void run() override {
        // App startup logic
    }
};

ENATIVE_REGISTER_TASK(StartupTask);
```

Tasks can self-destruct, be scheduled with delays, or persist as background services.

Use `eNative::TaskManager` to spawn concurrent workers without manual thread overhead:

```cpp
eNative::TaskManager::spawn([](){
    // Heavy async operation
});
```

For time-sensitive operations, use **Timed Tasks** to guarantee runtime slots:

```cpp
eNative::TaskManager::schedule([]() {
    // Called in 100ms
}, 100);
```

***

## 🚅 Performance Tips

To get the most from the eNative runtime, follow these:

| ✅ Do                                     | ❌ Avoid                              |
| ---------------------------------------- | ------------------------------------ |
| Use `ScopedAlloc` for short-lived memory | Leaving large memory in global scope |
| Prefer `TaskManager` over raw threads    | Manually managing long threads       |
| Use event-based logic                    | Constant polling or active loops     |
| Profile using `ewen profile --runtime`   | Guessing performance bottlenecks     |

For heavy apps, consider adding a **Resource Budget Manifest (RBM)** to tell the runtime what your app _expects_ to consume. This allows better optimization on EPU-class devices.

***

## 📈 Profiling & Diagnostics

Run your app in **Simulated Mode** with:

```bash
ewen simulate --device QA7 --profile
```

Or, inside EADS:

* Navigate to **Tools → Simulate & Profile**
* Select device target
* Run live with memory and CPU overlays

Use `EventView` to trace events and see which tasks cause latency or unnecessary calls.

***

## 🔚 Final Thoughts

The runtime is what gives eNative3 its power — but also its quirks. Understanding how it manages memory, executes tasks, and interfaces with hardware is essential to writing clean, stable, and futureproof software.

> **Next in the Deep Dive Series:**\
> → [event-driven-development-in-enative3.md](../event-driven-development-in-enative3.md "mention") — We’ll break down system events, user triggers, and custom event pipelines in modular apps.

