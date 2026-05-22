# 🎛️ Udon Graph Sidebar : Events & Groups Tabs

This document defines the strict rules for the `Events` and `Groups` tabs located in the left sidebar of the Udon Graph Editor (next to the Variables tab).

---

## ⚡ 1. The "Events" Tab (Custom Events)
This tab manages user-defined Custom Events within the graph. Unlike built-in VRChat events (Start, Interact), these are created by the developer to modularize logic or trigger actions over the network.

### 🤖 AI Strict Directives for Custom Events:
* **String-Based References:** Custom events are triggered via strings. The name defined in this tab must match EXACTLY the string used in a `SendCustomEvent` or `SendCustomNetworkEvent` node.
* **Naming Conventions:** Do NOT use spaces or special characters. Use PascalCase or camelCase (e.g., `UnlockDoor`, `PlaySound`).
* **CRITICAL RULE - NO PARAMETERS:** VRChat Udon Custom Events **cannot accept arguments or parameters**. 
  * ❌ *AI Hallucination:* Trying to call `SendCustomEvent("UpdateScore", 10)` will fail.
  * ✅ *Correct VRChat Method:* Set a specific `[UdonSynced]` or local variable first using a `Set Variable` node, and THEN call `SendCustomEvent("UpdateScore")`. The event will read the pre-set variable.
* **Network Execution:** Custom events defined here are the primary targets for RPCs (Remote Procedure Calls). When using `SendCustomNetworkEvent`, ensure you specify the target as either `All` or `Owner`.

---

## 📁 2. The "Groups" Tab (Node Organization)
This tab is used purely for visual organization and workspace management within the graph editor.

### 🤖 AI Rules for Groups:
* **Visual Only:** Groups have **zero impact** on the compiled C#/UdonAssembly code or execution flow. They exist strictly for developer readability.
* **Functionality:** Clicking `+` creates a resizable, colored background box that can contain multiple nodes.
* **Naming Suggestions:** AIs should recommend grouping complex node sequences to the user (e.g., "I recommend grouping these math nodes and naming the group 'Distance Calculation'").
* **No Logic Attachment:** Never attempt to target a "Group" with an execution flow or event. It is not a logical container, just a visual sticky note.
