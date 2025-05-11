# 📦 Mastering eNative Modules: Structure, Lifecycle, and Best Practices

**Applies to:** eNative3 RS8+ | EADS | EW-eN CLI | VS/VS Code Extensions\
**Skill Level:** Intermediate to Advanced

***

## 🚨 What are eNative Modules?

**eNative Modules** are the foundational building blocks of any eNative3 application. They’re lightweight, system-aware components written in C++ or natively accelerated languages that communicate directly with the eNative runtime and hardware layer.

Modules are used for:

* Logic encapsulation
* State management
* Interfacing with low-level hardware
* Exposing functionality to higher-level apps (including ROS or MatePC bridges)

> 🧠 Think of modules like "native microservices" for your app — ultra-performant, sandboxed, and reusable.

***

## 🧱 Module Anatomy

Every eNative module follows a predictable structure. Here’s a minimal example:

```cpp
#include <enative/module.h>

class MyModule : public eNative::Module {
public:
    void onInit() override {
        // Called when the module is loaded
    }

    void onEvent(const eNative::Event& event) override {
        // Handle events (hardware, system, or app-triggered)
    }

    void onDestroy() override {
        // Clean up resources
    }
};

ENATIVE_REGISTER_MODULE(MyModule)
```

***

## 🔄 Module Lifecycle

Understanding the lifecycle is key to optimizing performance and avoiding memory leaks.

| Stage         | Trigger                         | Purpose                           |
| ------------- | ------------------------------- | --------------------------------- |
| `onInit()`    | Module instantiation            | Initialization, memory allocation |
| `onEvent()`   | Incoming system or app event    | Event response, task scheduling   |
| `onDestroy()` | Module teardown or app shutdown | Resource deallocation, cleanup    |

You can also hook into optional lifecycle methods like `onSuspend()` and `onResume()` for background-aware apps.

***

## 📶 Event Handling in Practice

Events in eNative3 are highly customizable and efficient. Events are passed as structured objects containing metadata, type, and payload:

```cpp
void MyModule::onEvent(const eNative::Event& event) {
    if (event.type == "BatteryLow") {
        handleBattery(event.payload);
    }
}
```

***

## 📦 Packaging Modules

When you’re ready to ship, modules are compiled into `.emn` (Eco Module Native) bundles. These are signed automatically by EADS and are compatible across MateOS, ROS, DashEco, and most MatePC environments.

Using EW-eN CLI:

```bash
ewen build --module src/MyModule.cpp --out dist/MyModule.emn
```

In EADS:

1. Open your project.
2. Click **Build → Compile Module**.
3. EADS will output the `.emn` file to `/dist`.

***

## ✅ Best Practices

To keep your modules lightweight and scalable:

* **Avoid static memory unless absolutely necessary.** Use scoped memory allocation with `eNative::ScopedAlloc`.
* **Use hardware-specific directives** when targeting custom builds (e.g., `#if EPU5_HEXACORES`).
* **Never block `onEvent()`**. Delegate heavy work to threads or the Task Manager.
* **Document your module’s interface** if it’s reused by other teams or bridged into ROS.

***

## 🧩 Useful Tools

| Tool                            | Purpose                                                     |
| ------------------------------- | ----------------------------------------------------------- |
| `EADS Module Linter`            | Ensures compliance with eNative3 syntax and build standards |
| `EW-eN CLI Inspector`           | Debug and profile module performance                        |
| `VS Code eNative Insight Panel` | Visualizes memory use, event calls, and state transitions   |

***

## 🔚 Conclusion

Modules are where the real magic of eNative happens. They’re fast, portable, and capable of handling low-level operations that high-level frameworks can’t touch. By mastering module design and following best practices, you’ll be building scalable, futureproof applications that squeeze every drop of performance out of EcoWestern hardware.

> Dive deeper into Module Patterns in the **eNative3 Deep Dive Series →**

