# 🌟 Event-Driven Development in eNative3

**Applies to:** eNative3 RS1+\
**Skill Level:** Intermediate to Advanced\
**Last Updated:** May 2025

***

## 📘 Overview

In this second article of the _eNative3 Deep Dive Series_, we explore **Event-Driven Development** in eNative3 — a powerful paradigm that enables developers to write responsive, scalable, and modular applications. Whether you're building a lightweight mobile app or a high-performance system service, understanding how to properly use events in eNative3 will give your code incredible flexibility.

***

## 🔄 What is Event-Driven Development?

Event-driven development revolves around the idea that applications should react to events, such as user input, system signals, or changes in state, rather than relying on linear execution. In eNative3, this model is tightly integrated into the **task scheduler** and **runtime hooks**, allowing your app to process asynchronous operations without worrying about low-level threading or polling.

The runtime is designed to optimize event handling automatically, so developers can focus on defining what happens when an event occurs — rather than managing how it's processed.

***

## 🧩 Event Lifecycle in eNative3

In eNative3, every event is part of a **lifecycle** that includes these stages:

1. **Trigger:** The event is raised, either by the system or user interaction.
2. **Queue:** The event is added to an event queue, waiting to be handled.
3. **Execution:** The event is executed based on its priority and the available resources.
4. **Completion:** Once the event has been processed, it triggers any post-event tasks, such as callbacks or state updates.

***

## 🚀 Basic Event Handling

At the core of eNative3 event handling is the `eNative::Event` class, which allows you to define custom events and manage their lifecycles.

Here’s a basic example of an event handler for a simple user interaction:

```enative
#include <enative/event.h>

class UserEventHandler : public eNative::Event {
public:
    void onEvent() override {
        // Code to handle the user event
        eNative::Log::info("User event triggered!");
    }
};

// Register the event
ENATIVE_REGISTER_EVENT(UserEventHandler);
```

In this code:

* The `onEvent()` method is triggered whenever the event is fired.
* `ENATIVE_REGISTER_EVENT` registers the event within the runtime to be called when the event is triggered.

***

## 🔧 Using Event Handlers with `eNative::EventManager`

The `eNative::EventManager` is the central hub for managing all events. Developers can schedule events for execution, assign priorities, and manage callbacks all from this class.

Here’s an example of how to schedule and fire an event using `EventManager`:

```enative
#include <enative/event.h>

void fireCustomEvent() {
    // Fire the event with a high priority
    eNative::EventManager::fire<UserEventHandler>(eNative::EventPriority::High);
}

int main() {
    fireCustomEvent(); // Trigger the event
    return 0;
}
```

In this example:

* `eNative::EventManager::fire<T>` triggers the `UserEventHandler` event.
* Events are fired asynchronously, and `EventPriority` allows you to control their execution order.

***

## ⏳ Debouncing & Throttling Events

In real-world applications, events might be triggered multiple times in quick succession (e.g., user clicks, sensor readings). To avoid overloading your system with unnecessary tasks, **debouncing** and **throttling** are important strategies for managing event frequency.

Here’s an example of **debouncing** an event (only firing once after a delay):

```enative
#include <enative/event.h>
#include <enative/timer.h>

class DebouncedEventHandler : public eNative::Event {
private:
    eNative::Timer debouncer;

public:
    void onEvent() override {
        // Debounce: wait 100ms before firing the event again
        if (!debouncer.isRunning()) {
            debouncer.start(100, [this](){
                eNative::Log::info("Event fired after debounce!");
            });
        }
    }
};

// Register the debounced event
ENATIVE_REGISTER_EVENT(DebouncedEventHandler);
```

In this example:

* A **timer** is used to delay the event, ensuring it doesn’t fire repeatedly in a short time span.
* Debouncing is useful in situations like handling button presses or input field changes.

***

## 🔗 Event Chains & Callbacks

Events in eNative3 can trigger other events, creating an event chain. This is useful for building complex workflows. You can also attach **callbacks** to events, specifying what happens once an event completes.

Here’s an example of triggering a second event after the first one completes:

```enative
#include <enative/event.h>

class FirstEventHandler : public eNative::Event {
public:
    void onEvent() override {
        eNative::Log::info("First event completed. Firing second event...");
        
        // Fire the second event after the first finishes
        eNative::EventManager::fire<SecondEventHandler>();
    }
};

class SecondEventHandler : public eNative::Event {
public:
    void onEvent() override {
        eNative::Log::info("Second event completed!");
    }
};

// Register events
ENATIVE_REGISTER_EVENT(FirstEventHandler);
ENATIVE_REGISTER_EVENT(SecondEventHandler);
```

This creates a sequence of events:

1. **FirstEventHandler** runs, then triggers **SecondEventHandler**.
2. The completion of one event causes the next one to trigger.

***

## 📈 Optimizing Event Processing

Although eNative3’s event-driven architecture is highly efficient, poor event design can still degrade performance. Here are some tips for optimizing your event system:

| ✅ Do                                              | ❌ Avoid                                                   |
| ------------------------------------------------- | --------------------------------------------------------- |
| Minimize event chaining (limit depth)             | Overloading the event queue with high-priority events     |
| Use event pooling for frequently triggered events | Using global variables in event handlers                  |
| Handle errors gracefully within event handlers    | Blocking the event thread (e.g., through synchronous I/O) |
| Keep event handlers small and focused             | Creating too many long-running background tasks           |

Additionally, utilize **event profiling** in the EADS simulator to track the performance of individual events in your app.

***

## 🚨 Error Handling in Events

Like any system, events can fail. Using the built-in error handling mechanisms in eNative3, you can catch exceptions inside event handlers and ensure that the rest of the system continues to function smoothly:

```enative
#include <enative/event.h>

class SafeEventHandler : public eNative::Event {
public:
    void onEvent() override {
        try {
            // Code that might fail
            throw std::runtime_error("Something went wrong!");
        } catch (const std::exception& e) {
            eNative::Log::error("Event error: " + std::string(e.what()));
        }
    }
};

// Register the event
ENATIVE_REGISTER_EVENT(SafeEventHandler);
```

In this example:

* If an error occurs inside the event handler, it is caught and logged, preventing the event system from crashing.

***

## 🧠 Final Thoughts

Event-driven development is a cornerstone of building responsive, modular applications in eNative3. By leveraging the power of **task-based scheduling**, **event chains**, **debouncing**, and **callbacks**, you can create highly efficient, maintainable, and scalable systems. The more you understand how events work within the eNative3 runtime, the more control you have over how your applications behave in real-world scenarios.

> **Next in the Deep Dive Series:**\
> → _Task Scheduling and Performance Optimization_ — We’ll take a closer look at managing task priorities, optimizing execution cycles, and reducing overhead in high-performance apps.

