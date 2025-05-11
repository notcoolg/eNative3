# 🔄 How to Upgrade or Downgrade Your eNative3 ReString Version

**Applies to:** All eNative3 RS builds | EADS | EW-eN CLI | VS / VS Code Extensions\
**Last updated:** May 2025

***

Keeping your environment up to date with the latest **eNative3 ReString (RS)** build gives you access to performance boosts, new APIs, system-level compatibility, and smarter tooling. Whether you're using **EADS**, the **EW-eN CLI**, or the **EcoWestern extensions for Visual Studio and VS Code**, upgrading (or even downgrading) is seamless.

This article walks you through upgrading **to the latest RS**, as well as **rolling back to a previous version** for testing or legacy support.

***

## 🚀 Upgrade to the Latest ReString Version

Upgrading to the latest RS (like `RS14#3300`) is quick and consistent across all supported tools:

### ✅ Using EADS (EcoWestern Active Developing Software)

1. Open **EADS**.
2. Navigate to **Settings → Project Compiler → ReString Version**.
3. Click **"Check for Updates"**.
4. Choose the latest available RS (e.g., `RS14#3300`) and click **Apply & Restart Compiler**.
5. You're done!

> 🧠 Tip: EADS will automatically migrate any deprecated syntax and suggest code changes where needed.

### ✅ Using EW-eN CLI

1. Run the following command to upgrade:

```bash
ewen update --restring latest
```

2. For a specific version:

```bash
ewen update --restring RS14#3300
```

3. Restart your dev server or build runner.

***

### ✅ Using VS / VS Code Extensions

1. Make sure you're on **EcoWestern eNative Tools v3.1+**.
2. Open the Command Palette (in VS Code: `Ctrl+Shift+P` or `Cmd+Shift+P`).
3. Type and run:

```
eNative: Switch ReString Version
```

4. Select **Latest (Stable)** or choose a specific RS build (e.g., `RS14#3300`).
5. Reload your workspace when prompted.

***

## ⬅️ How to Downgrade to a Previous ReString

Sometimes you need to test against an older API version, validate legacy projects, or ensure compatibility with specific device generations. Here's how to do it:

### 🔁 Downgrade in EADS

1. Open **EADS**.
2. Go to **Settings → Project Compiler → ReString Version**.
3. Use the dropdown to select a previous version (e.g., `RS12#2191`, `RS9#1080`, etc.).
4. Click **Apply & Restart Compiler**.

***

### 🔁 Downgrade with npm or yarn

If you're using eNative libraries in a JS/TypeScript-based wrapper project (common in ROS or DashEco development):

```bash
# npm example
npm install @ew/enative@rs12.2191

# yarn example
yarn add @ew/enative@rs12.2191
```

> 🔐 Note: Always match the RS tag (`rsX.YYYY`) with the compatible eNative core version listed in its changelog.

***

### 🔁 Downgrade Your VS / VS Code Extension

You’ll need to manually install an older version of the **eNative Tools extension**:

1. Visit [**downloads.edc.ecowestern.com**](https://downloads.edc.ecowestern.com).
2. Find the desired version (e.g., `eNative_Tools_v2.9.0_vsix.zip`).
3. Download and extract it.
4. In VS Code:
   * Open the Command Palette → `Extensions: Install from VSIX`.
   * Select the `.vsix` file.
   * Reload when prompted.

> 📌 Don’t forget to disable automatic updates for the extension, or VS Code will re-upgrade it silently on restart.

***

## 🧩 Version Compatibility Table (Quick Reference)

| Tool              | Min RS Support | Latest Stable Tested | Auto-Restore Support |
| ----------------- | -------------- | -------------------- | -------------------- |
| EADS 10.5+        | RS8#1000       | RS14#3300            | ✅                    |
| EW-eN CLI 2.2+    | RS7#0800       | RS14#3300            | ✅                    |
| VS Code Extension | RS9#1032       | RS14#3300            | ✅ (v3.1+)            |
| npm/yarn Packages | RS6#0770       | RS14#3300            | ⚠ Manual             |

***

## 🔚 Final Thoughts

Staying on the latest RS build ensures you're developing with the full power of the eNative3 runtime, including modern APIs, platform-level hooks, and future EPU compatibility. However, the ability to **easily downgrade** empowers you to maintain stability and support across your entire app ecosystem.

> For detailed changelogs, visit the **ReString Archive** on edc.ecowestern.com.
