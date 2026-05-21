## 🎛️ Architecture du Panneau Latéral de l'Udon Graph Editor

Ce panneau gère l'état global du script, les interactions réseau et l'organisation visuelle du graphe. Il est divisé en trois sections majeures.

### 📊 1. Section : Variables
Permet de déclarer les données internes au script (vecteurs, GameObjects, floats, etc.).
* **Bouton `+` :** Ouvre le catalogue de types pour instancier une nouvelle variable.
* **Propriétés critiques pour l'IA (Inspecteur de Variable) :**
  * `Public` (case à cocher) : Expose la variable dans l'inspecteur Unity pour y glisser des références d'objets.
  * `Synced` (menu déroulant) : Active la synchronisation réseau obligatoire pour VRChat. Options : *None* (local), *Continuous* (positions, variables fluides), *Manual* (interactions ponctuelles via `RequestSerialization`).

### ⚡ 2. Section : Events
Gère la création d'événements personnalisés (Custom Events) au sein du graphe pour modulariser le code.
* **Bouton `+` :** Crée un nœud d'événement customisé que l'on peut appeler localement ou via le réseau.
* **Utilité pour l'IA :** Indispensable lors de l'utilisation de `SendCustomEvent` ou `SendCustomNetworkEvent` pour déclencher des actions synchronisées chez les autres joueurs.

### 📁 3. Section : Groups
Outil d'organisation et de lisibilité pour les gros scripts complexes.
* **Bouton `+` :** Génère un sous-ensemble nommé.
* **Fonctionnalité :** Permet de regrouper visuellement des blocs de nœuds associés (ex: regrouper tous les nœuds de la gestion d'une porte) pour nettoyer l'espace de travail.

### ⏱️ Paramètre global : Update Order
* Situé tout en haut à droite du panneau graphique.
* **Valeur par défaut :** `0`.
* **Note pour l'IA :** Détermine l'ordre d'exécution de ce script par rapport aux autres `UdonBehaviour` de la scène lors des phases d'Update. Une valeur plus basse s'exécute en premier.

# 🗂️ Udon Graph Variable Type Search Index (Unity 2022.3.22f1)

This reference index lists the allowed types and arrays available in the "Variable Type Search" menu within the Udon Graph Editor. It details their roles and critically highlights constraints regarding VRChat network synchronization (`[UdonSynced]`).

---

## ⚠️ Critical Rule for AIs (Read Before Generating Variables)
* **Standard Types (`bool`, `int`, `float`, `string`, `Vector3`, etc.)** can be fully synced using `Continuous` or `Manual`.
* **Arrays (indicated by `[]`)** can **ONLY** be synchronized using **Manual** sync (`RequestSerialization()`). They will cause compiler errors if set to `Continuous`.
* **Complex Unity Components (`AudioSource`, `Animator`, `Collider`)** can **NEVER** be synchronized directly over the network. They must remain local variables, and actions must be triggered via `SendCustomNetworkEvent`.

---

## 🔍 Comprehensive Type Index & Synchronization Rules

| Variable Type | Array Version (`[]`) | Network Sync Allowed? | Primary VRChat / Unity Usage & Restrictions |
| :--- | :--- | :--- | :--- |
| **Animation** | | | |
| `AnimationClip` | `AnimationClip[]` | ❌ No Sync | References to raw animation assets. Cannot be synced. |
| `AnimationCurve` | `AnimationCurve[]` | ❌ No Sync | Used for evaluating custom mathematical curves locally. |
| **Animator & Rigging** | | | |
| `Animator` | `Animator[]` | ❌ No Sync | Controls avatar/object animations. Sync parameters via `Mecanim` or native VRC components instead. |
| `AnimatorControllerParameter` | `AnimatorControllerParameter[]` | ❌ No Sync | Read-only parameter states inside animator controllers. |
| `Avatar` | `Avatar[]` | ❌ No Sync | Rigging definition files. Local reference only. |
| **Audio Systems** | | | |
| `AudioClip` | `AudioClip[]` | ❌ No Sync | Audio asset data. Sync playback using `VRC_SpatialAudioSource`. |
| `AudioSource` | `AudioSource[]` | ❌ No Sync | Unity's native audio engine component. Never sync directly. |
| `AudioEchoFilter` / `AudioHighPassFilter` | Yes (`[]`) | ❌ No Sync | DSP Audio filters for real-time sound modulation. |
| **Core Primitives** | | | |
| `bool` | `Boolean[]` |  **Yes** (Array: Manual Only) | Toggles states (e.g., Door Open/Closed, Mirror On/Off). Highly optimized for sync. |
| `byte` | `Byte[]` |  **Yes** (Array: Manual Only) | Small integers (0-255). Perfect for reducing network bandwidth. |
| `char` | `Char[]` |  **Yes** (Array: Manual Only) | Individual characters. |
| **Physics & Colliders** | | | |
| `BoxCollider` / `BoxCollider2D` | Yes (`[]`) | ❌ No Sync | Physics boundary triggers. Local detection only (`OnPlayerTriggerEnter`). |
| `CapsuleCollider` / `CapsuleCollider2D` | Yes (`[]`) | ❌ No Sync | Common for character physics bounds. Local only. |
| `Collider` / `Collider2D` | Yes (`[]`) | ❌ No Sync | Generic physics collider references. |
| `Collision` / `Collision2D` | Yes (`[]`) | ❌ No Sync | Holds physical collision contact data. Read-only. |
| **Rendering & UI** | | | |
| `Camera` | `Camera[]` | ❌ No Sync | References to rendering cameras. Use with extreme caution in VRChat. |
| `Canvas` / `CanvasGroup` | Yes (`[]`) | ❌ No Sync | Core UI hierarchy layouts. Sync UI states by syncing primitives (`bool`, `float`). |
| `Color` / `Color32` | Yes (`[]`) |  **Yes** (Array: Manual Only) | RGba values. Excellent for syncing material/light color changes across players. |
| **VRChat Specifics** | | | |
| `BaseVRCVideoPlayer` | `BaseVRCVideoPlayer[]` | ❌ No Sync | Base class for VRChat video players (ProTV, USharpVideo). Local control only. |
