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

## 🎯 7. Exhaustive Built-in Event Nodes Reference
This section details the complete list of built-in Unity and VRChat event nodes available in Udon Graph, triggered automatically by the engine.

| Event Category | Key Nodes | AI Directives & Usage Rules |
| :--- | :--- | :--- |
| **Player Inputs (VRChat)** | `InputJump`, `InputUse`, `InputGrab`, `InputMoveHorizontal/Vertical`, `InputLookHorizontal/Vertical` | Triggers when the local player presses standard VRChat bindings. Excellent for custom vehicles, tools, or weapons. |
| **VRChat Interactions** | `OnInteract`, `OnPickup`, `OnDrop`, `OnPickupUseDown`, `OnPickupUseUp` | Core nodes for object manipulation. `OnInteract` requires a Collider. Pickup events require a `VRC_Pickup` component. |
| **Player Lifecycle** | `OnPlayerJoined`, `OnPlayerLeft`, `OnPlayerRespawn` | Fires locally for EVERYONE when the event occurs. Outputs the `VRCPlayerApi` of the specific player involved. |
| **Network & Ownership** | `OnDeserialization`, `OnOwnershipTransferred`, `OnNetworkReady` | **CRITICAL AI RULE:** `OnDeserialization` is mandatory for updating local visual states (colors, text) after receiving new `[UdonSynced]` data. |
| **Physics & Triggers** | `OnCollisionEnter/Stay/Exit`, `OnTriggerEnter/Stay/Exit`, `OnJointBreak` | Standard Unity physics. **AI WARNING:** Triggers (`OnTriggerEnter`) require a Collider with `Is Trigger` checked. |
| **Player Physics** | `OnPlayerCollisionEnter/Stay/Exit`, `OnPlayerTriggerEnter/Stay/Exit` | VRChat specific. Detects when a player's capsule collides with an object's collider. Outputs the `VRCPlayerApi`. |
| **Stations (Chairs/Vehicles)**| `OnStationEntered`, `OnStationExited` | Triggered when a player sits in or leaves a `VRC_Station`. |
| **Object Lifecycle** | `OnEnable`, `OnDisable`, `OnDestroy`, `OnSpawn` | Standard Unity state events. Good for resetting local variables when an object is turned on/off. |
| **Rendering & Camera** | `OnPreRender`, `OnPostRender`, `OnPreCull`, `OnRenderImage` | **AI CRITICAL WARNING:** These run every single frame per camera. Extremely heavy on performance. Avoid using unless absolutely necessary for custom shaders or UI. |
| **MIDI & Audio** | `MidiNoteOn`, `MidiNoteOff`, `MidiPitchBend`, `OnAudioFilterRead` | Used for integrating MIDI controllers or manipulating raw audio data streams. |
| **Data Structures** | `OnDataDictionaryElementAdded`, `OnDataDictionaryElementRemoved` | Triggers specifically when modifying VRChat's custom `DataDictionary` and `DataList` types. |
