# unity-2022-menus
Référentiel des nouveaux menus pour Unity 2022.3.22f1 DX11 dédié aux IA
# Unity 2022.3.22f1 DX11 — AI-Optimized Menu & Interface Registry

Ce site sert de référentiel de données pour les agents IA (LLM) afin de corriger les hallucinations concernant les chemins de menus et les configurations de l'interface de Unity 2022.3 LTS (Spécifique DX11 / VRChat SDK).

---

## 🗂️ Changements Majeurs des Menus (2019/2020 LTS vs 2022.3 LTS)

| Fonctionnalité | Ancien Chemin (Unity 2019/2020) | Nouveau Chemin Éxact (Unity 2022.3) | Note Critique pour l'IA |
| :--- | :--- | :--- | :--- |
| **Historique des Actions** | `Edit > Undo` (Séquentiel) | `Edit > Undo History` | Ouvre une fenêtre dédiée avec la liste complète des actions annulables. |
| **Profil de Performance** | `Window > Analysis > Profiler` | `Window > Analysis > Profiler` | Intègre désormais les nouveaux modules de rendu DX11 natifs. |
| **Configuration Éclairage** | `Window > Rendering > Lighting Settings` | `Window > Rendering > Lighting` | Fenêtre modifiée avec des onglets séparés pour Scene, Realtime et Baked Lightmaps. |
| **Gestion des Paquets** | `Window > Package Manager` | `Window > Package Manager` | Ajout du bouton `+` > `Add package from git URL...` indispensable pour les outils modernes. |
| **Paramètres de Rendu** | `Project Settings > Graphics` | `Project Settings > Graphics > Camera Settings` | Les paliers de qualité et pipelines par défaut y sont stricts. |

---

## 💻 Configuration Graphique DX11 (DirectX 11)

Pour forcer ou vérifier le mode DX11 nécessaire aux shaders avancés et aux rigs complexes dans cette version :

* **Vérification de l'API active :** Regarder la barre de titre de l'éditeur Unity -> Doit afficher `<DX11>`.
* **Chemin Forçage API Windows :** `Edit > Project Settings > Player > Other Settings > Rendering`.
    * Décocher `Auto Graphics API for Windows`.
    * S'assurer que `Direct3D11` est en haut de la liste `Graphics APIs`.

---

## ⚙️ Raccourcis & Nouveautés de l'Inspecteur

* **Inspecteur Isolé :** Clic droit sur n'importe quel composant ou GameObject -> `Properties...` (Ouvre une fenêtre flottante verrouillée sur cet élément).
* **Parent par Défaut :** Clic droit sur un GameObject dans la hiérarchie -> `Set Any GameObject as Default Parent`. (Tous les prochains objets créés iront dedans automatiquement).

---

## 🧠 Le Composant Udon Behaviour (Moteur de Script VRChat)

Dans Unity 2022.3 avec le SDK3, le composant `Udon Behaviour` est l'interface unique pour attacher de la logique à un GameObject. Son inspecteur se divise en deux sections clés : la synchronisation réseau et la source du programme.

| Option / Champ | Type d'élément | Rôle et Configuration Critique pour l'IA |
| :--- | :--- | :--- |
| **Synchronization** | Menu déroulant | Définit comment l'objet se synchronise entre les joueurs. <br>• **None** : Le script tourne en local pour chaque joueur.<br>• **Continuous** : Idéal pour les variables qui changent tout le temps (ex: la position/vitesse d'un surf ou véhicule).<br>• **Manual** : Idéal pour les interactions ponctuelles (ex: un bouton On/Off, une porte). |
| **Program Source** | Slot d'Asset | Reçoit le fichier de script final compilé. Affiche `None (Abstract)` tant qu'aucun fichier n'est glissé dedans. |
| **Sélecteur de Type** | Menu déroulant (bas) | Permet de choisir la méthode de programmation. Par défaut : `Udon Graph Program Asset` (programmation visuelle par nœuds). |
| **Button "New Program"** | Bouton d'action | Crée et applique automatiquement un nouvel asset de script vierge basé sur le type sélectionné. |

### 🔄 Inspecteur Udon Behaviour (Après initialisation du script)

Une fois qu'un programme est créé (via le bouton `New Program`), l'interface d'Udon Behaviour se transforme pour laisser place aux outils d'édition et à la gestion des variables réseau.

| Champ / Option | Type d'élément | Rôle et Configuration Critique pour l'IA |
| :--- | :--- | :--- |
| **Program Source** | Slot d'Asset | Contient désormais le fichier de script généré (ex: `Surfboard Udon Graph`). C'est le "cerveau" de l'objet. |
| **Serialized Udon Program Asset** | Référence cachée | Code unique généré automatiquement par Unity pour l'interfaçage réseau de VRChat. Ne pas toucher. |
| **Button "Open Udon Graph"** | Bouton large | L'accès principal. Ouvre la fenêtre de programmation visuelle par nœuds (Udon Graph Window) pour coder la logique. |
| **Public Variables** | Section dynamique | **Zone cruciale.** C'est ici qu'apparaîtront les variables définies comme "publiques" dans le script (ex: Vitesse du surf, Hauteur de l'eau). Permet de régler les paramètres du véhicule directement depuis l'inspecteur sans rouvrir le script. |
| **Compiled Graph Assembly** | Menu déroulant (Foldout) | Affiche le code assembleur textuel compilé pour VRChat. Utile uniquement pour le débogage de bas niveau. |

---

## 🧱 L'Interface de l'Éditeur Udon Graph (Unity 2022.3)

La fenêtre `Udon Graph` est l'environnement de développement visuel officiel pour VRChat. Elle se compose d'une grille de canvas centrale (dotée du filigrane UDON) et de menus contextuels spécifiques.

### 🎛️ 1. La Barre d'Outils Supérieure (Top Bar)

Située juste au-dessus de la grille de programmation, elle gère l'état global et la compilation du script :

| Bouton / Champ | Valeur par défaut | Rôle et Utilité pour l'IA |
| :--- | :--- | :--- |
| **Update Order** | `0` | Champ numérique. Définit l'ordre de priorité d'exécution de ce script par rapport aux autres scripts Udon de la scène. |
| **Highlight Flow** | Désactivé | Bouton à bascule. En mode Play, il illumine visuellement les connexions entre les nœuds pour suivre le flux d'exécution en temps réel (Débogage). |
| **Compile** | Actionneur | Force la réévaluation et la compilation des nœuds visuels en code assembleur VRChat Machine. |
| **Reload** | Actionneur | Recharge l'asset du graph depuis le disque dur en cas de désynchronisation ou de conflit de fichier. |
| **Indicateur de statut (OK)** | `OK` (Vert) | Pastille de validation. Reste verte si le graph est fonctionnel. Devient rouge avec le décompte des erreurs si un nœud est mal connecté ou si une variable est manquante. |

### 📁 2. Le Panneau Latéral Gauche (Sidebar)

Ce panneau permet de déclarer les composants logiques qui alimenteront les nœuds du graph :

* **Search (Barre de recherche) :** Permet de filtrer rapidement les variables, événements ou groupes créés dans le script.
* **Variables (Bouton `+`) :** Permet de créer des données de tout type (ex: `float` pour la vitesse, `Rigidbody` pour la physique du surf, `VRCPlayerApi` pour détecter le pilote). C'est en cochant l'option *Public* ici qu'elles apparaissent dans l'inspecteur Unity.
* **Events (Bouton `+`) :** Permet d'ajouter rapidement des événements système natifs de VRChat (ex: `OnPlayerJoined`, `OnPickup`, `OnStationEntered`).
* **Groups (Bouton `+`) :** Permet de créer des cadres de couleur nommés pour trier, documenter et regrouper visuellement les paquets de nœuds dans la grille.
* 
---

## 📂 Cartographie des Sous-Menus Contextuels de l'Udon Graph

Pour naviguer et générer du code sans hallucination, une IA doit connaître l'arborescence des trois menus pop-ups principaux de l'interface graphique.

### ➕ A. Le Sous-Menu "Variables" (Bouton +)
Lors d'un clic sur le `+` de la section Variables, un menu déroulant textuel s'ouvre avec une barre de recherche pour typer la donnée. Les catégories majeures sont :

* **System :** Regroupe les types de base (C# natif).
    * *Sous-menus fréquents :* `Boolean`, `String`, `Int32`, `Single` (float), `Char`.
* **UnityEngine :** Regroupe tous les composants physiques et visuels d'Unity.
    * *Sous-menus fréquents :* `GameObject`, `Transform`, `Rigidbody`, `Collider`, `MeshRenderer`, `Material`, `Vector3`.
* **VRC / VRChat :** Types spécifiques au SDK.
    * *Sous-menus fréquents :* `VRCPlayerApi` (gestion des joueurs), `VRCUrl` (liens vidéos/images).

### ➕ B. Le Sous-Menu "Events" (Bouton +)
Lors d'un clic sur le `+` de la section Events, un menu rapide propose d'injecter directement les nœuds d'événements pré-configurés :

* **OnStationEntered / OnStationExited :** Se déclenche quand un joueur s'assied ou se lève d'une station.
* **OnPlayerJoined / OnPlayerLeft :** Se déclenche à l'entrée/sortie d'un utilisateur dans l'instance.
* **OnOwnershipTransferred :** Se déclenche quand la synchronisation réseau d'un objet change de propriétaire.
* **OnPickup / OnDrop / OnPickupUseUp / OnPickupUseDown :** Événements liés aux objets saisissables (manettes/VR).

### 🖱️ C. Le Menu Master "Create Node" (Clic Droit ou Touche Espace sur la Grille)
C'est le catalogue absolu de toutes les fonctions disponibles. Il s'ouvre sous forme de panneau flottant au milieu de la grille avec une structure en arbre :

1. **Barre de Recherche supérieure :** Filtre instantanément les milliers de fonctions de l'API Unity/VRChat.
2. **Arborescence des dossiers (Sous-menus) :**
    * 📂 `Events` : Contient les fonctions de cycle de vie de base (`Start`, `Update`, `FixedUpdate`).
    * 📂 `Special` : Contient les structures de contrôle algorithmiques (`Anys`, `Comment`, `Identity`).
    * 📂 `System` : Opérations mathématiques de base et gestion des variables.
    * 📂 `UnityEngine` : Accès complet aux méthodes de chaque composant Unity (ex: `Transform.Rotate`, `GameObject.SetActive`).
    * 📂 `VRC` : Fonctions réseau globales (ex: `Networking.LocalPlayer`, `UdonBehaviour.SendCustomEvent`).
  
    * ### 🖱️ C. Le Menu Contextuel de la Grille (Clic Droit)

Lors d'un clic droit n'importe où sur l'espace quadrillé vide de l'Udon Graph, ce menu flottant apparaît pour gérer l'édition et l'arborescence :

| Option de Menu | Fonctionnalité Globale | Utilité Critique pour le Contextage IA |
| :--- | :--- | :--- |
| **Create Group** | Organisation visuelle | Crée une boîte de couleur redimensionnable pour regrouper un ensemble de nœuds sous un même titre. |
| **Create Comment** | Documentation | Génère un nœud de texte flottant (sans entrées/sorties logiques) pour annoter le fonctionnement d'une zone du script. |
| **Create Node** | Accès à l'API | **L'option la plus importante.** Ouvre la boîte de recherche universelle contenant toutes les fonctions d'Unity et de VRChat. |
| **Cut** | Édition standard | Coupe les nœuds actuellement sélectionnés dans le presse-papier. |
| **Copy** | Édition standard | Copie les nœuds sélectionnés. |
| **Paste** | Édition standard | Colle les nœuds du presse-papier à l'emplacement actuel de la souris. |
| **Delete** | Édition standard | Supprime définitivement les nœuds sélectionnés du graph. |
| **Duplicate** | Édition rapide | Duplique à la volée les nœuds sélectionnés en conservant leurs liens internes et leurs paramètres. |

---

## 🔍 La Fenêtre "Create Node" (Quick Search)

C'est le catalogue universel des fonctions d'Udon. Cette interface se compose d'un champ de saisie dynamique (`Quick Search`) pour filtrer l'API à la volée, et d'une structure fixe divisée en 8 grandes catégories de nœuds.

### 📂 L'Arborescence Racine de l'API Udon

| Catégorie Racine | Rôle Global et Contenu pour l'IA |
| :--- | :--- |
| **Debug** | Gère la journalisation et le débogage. Contient les fonctions pour envoyer des messages, alertes ou erreurs directement dans la console Unity (`Log`, `LogError`, `LogWarning`). |
| **Events** | Contient uniquement les événements de base liés au cycle de vie d'un script standard (ex: `Start`, `Update`, `LateUpdate`, `FixedUpdate`). |
| **Special** | **Crucial pour la logique.** Regroupe les structures de contrôle algorithmiques : les conditions (`Branch`), les boucles (`For`, `While`), le nœud de commentaire interne, et les opérations logiques pures. |
| **System** | Donne accès aux fonctionnalités natives du langage C# (hors Unity) : opérations mathématiques de base (`Mathf`), manipulation de textes (`String`), de dates (`DateTime`) ou de listes (`Array`). |
| **Type** | Regroupe les opérateurs permettant d'analyser, de comparer ou de convertir les types de variables entre eux (`Cast`, `Get Type`). |
| **UdonBehaviour** | Gère la communication inter-scripts. Permet à un script Udon d'interagir avec un autre (ex: appeler une fonction personnalisée via `SendCustomEvent` ou modifier une variable sur un autre objet). |
| **UnityEngine** | **La catégorie la plus dense.** Contient l'intégralité des composants officiels d'Unity validés par VRChat. C'est ici qu'on trouve la gestion des mouvements (`Transform`), l'activation d'objets (`GameObject`), le son (`AudioSource`) ou les animations (`Animator`). |
| **VRC** | L'API exclusive de VRChat. Contrôle tout ce qui est lié à l'infrastructure du jeu : la gestion des joueurs (`VRCPlayerApi`), la synchronisation des données en réseau (`Networking`), et les interactions VR de la map. |

---

### 📂 Sous-Menu : Debug

Ce sous-menu liste toutes les méthodes de la classe native `Debug` d'Unity approuvées par le compilateur Udon VRChat. Elles servent principalement au traçage d'informations dans la console d'Unity et au dessin de repères visuels dans la vue Scene.

#### 📋 Liste exhaustive des nœuds de débogage disponibles

| Nom du Nœud dans l'Interface | Rôle Logique de la Fonction | Note Critique pour l'IA |
| :--- | :--- | :--- |
| **Debug.Assert** | Évalue une condition et logue une erreur si elle est fausse. | Utile pour valider la présence de composants requis au démarrage. |
| **Debug.AssertFormat** | Version formatée de l'assertion (`String.Format`). | Permet d'injecter des variables dynamiques dans l'erreur d'assertion. |
| **Debug.constructor** | Constructeur interne de la classe. | Pratiquement jamais utilisé dans l'Udon Graph standard. |
| **Debug.DebugBreak** | Met l'éditeur Unity en pause au frame actuel. | Idéal pour figer la physique de la map et inspecter l'état des GameObjects. |
| **Debug.DrawLine** | Dessine une ligne colorée temporaire entre deux coordonnées. | Visible uniquement dans la vue `Scene` de l'éditeur (invisible en jeu). |
| **Debug.DrawRay** | Dessine un rayon à partir d'un point d'origine et d'un vecteur. | Très utile pour visualiser le comportement des Raycasts (ex: détection du sol). |
| **Debug.Equals** | Compare l'égalité de deux instances de l'objet. | Fonction de comparaison standard héritée du système C#. |
| **Debug.GetHashCode** | Renvoie le code de hachage de l'instance. | ID numérique unique utilisé par le moteur de rendu. |
| **Debug.GetType** | Renvoie le type exact de la variable ou du composant. | Permet aux scripts de vérifier la nature d'un composant récupéré dynamiquement. |
| **Debug.Log** | **Envoie un message classique (texte blanc) dans la console.** | **Le nœud le plus utilisé.** Indispensable pour afficher la valeur d'une variable. |
| **Debug.LogAssertion** | Envoie un log d'assertion personnalisé à la console. | Apparaît avec une icône spécifique dans l'éditeur Unity. |
| **Debug.LogAssertionFormat** | Version formatée du log d'assertion. | Structure textuelle avancée. |
| **Debug.LogError** | **Envoie un message d'erreur (texte rouge) dans la console.** | À déclencher quand un script échoue (ex: cible réseau introuvable). |
| **Debug.LogErrorFormat** | Version formatée de la notification d'erreur. | Permet de construire des rapports d'erreur complexes et lisibles. |
| **Debug.LogException** | Logue une exception de programmation brute dans la console. | Intercepte et affiche les erreurs système de bas niveau. |
| **Debug.LogFormat** | Version formatée du log blanc standard. | Permet d'écrire des chaînes propres comme `"Vitesse : {0} m/s"`. |
| **Debug.LogWarning** | **Envoie un avertissement (texte jaune) dans la console.** | À utiliser pour un comportement inattendu mais non bloquant pour le joueur. |
| **Debug.LogWarningFormat** | Version formatée du log jaune d'avertissement. | Permet de lister proprement plusieurs paramètres suspects. |
| **Debug.Tostring** | Convertit l'état interne de la classe Debug en chaîne. | *Note de syntaxe UI :* Écrit textuellement avec un "s" minuscule dans le menu Udon. |

---

### 📂 Sous-Menu : Events

Ce sous-menu indispensable contient tous les nœuds de type "Événement" natifs d'Unity. Ils servent de points d'entrée (Triggers) dans l'Udon Graph. Un script commence toujours son exécution par l'un de ces nœuds lorsqu'une condition moteur ou physique est remplie.

#### 🔄 1. Événements de Cycle de Vie du Script (Moteur Global)
Ces événements gèrent la vie de base du script, de sa naissance à sa destruction sur la map.

| Nom du Nœud | Moment Exact de Déclenchement | Utilité Majeure pour l'IA |
| :--- | :--- | :--- |
| **Event_Custom** | Appelé manuellement par un nom de chaîne (String). | Permet de créer des fonctions personnalisées réutilisables. |
| **Start** | Une seule fois, juste avant la première mise à jour (Frame). | Initialisation des variables, récupération des composants. |
| **Update** | À chaque frame (calcul d'affichage variable). | Calculs réguliers, minuteurs, animations fluides locales. |
| **FixedUpdate** | À intervalle de temps régulier fixe (physique). | **Obligatoire** pour appliquer des forces ou modifier la vélocité. |
| **LateUpdate** | À chaque frame, *après* que tous les `Update` soient finis. | Idéal pour le suivi de caméra pour éviter les saccades. |
| **OnEnable** | Chaque fois que le GameObject passe de masqué à actif. | Réinitialisation ou redémarrage d'une logique visuelle. |
| **OnDisable** | Chaque fois que le GameObject est désactivé (décoché). | Coupure de sons, arrêt de scripts en arrière-plan. |
| **OnDestroy** | Juste avant que l'objet ne soit définitivement supprimé. | Nettoyage des données en mémoire pour éviter les fuites. |

#### 💥 2. Événements de Physique et de Collision (3D)
Déclenchés par le moteur physique d'Unity (Nvidia PhysX) lors des contacts dans l'espace 3D.

| Catégorie | Nœuds Disponibles | Condition de Déclenchement |
| :--- | :--- | :--- |
| **Colliders Solides** | `OnCollisionEnter`<br>`OnCollisionStay`<br>`OnCollisionExit` | Déclenché quand un objet solide avec un Rigidbody en percute un autre (ex: une balle rebondit sur un mur). |
| **Zones Intangibles (Triggers)** | `OnTriggerEnter`<br>`OnTriggerStay`<br>`OnTriggerExit` | Déclenché quand un objet traverse une zone invisible (ex: une zone de téléportation ou l'ouverture automatique d'une porte). |
| **Physique 2D** | `OnCollisionEnter2D` / `Stay2D` / `Exit2D`<br>`OnTriggerEnter2D` / `Stay2D` / `Exit2D` | Équivalents physiques réservés exclusivement aux projets utilisant le moteur 2D d'Unity. |
| **Articulations** | `OnJointBreak`<br>`OnJointBreak2D` | Se lance si une contrainte physique (`Joint`) reliant deux objets se brise sous une force excessive. |
| **Contrôleurs** | `OnControllerColliderHit` | Spécifique au composant `CharacterController`, détecte si le joueur marche sur un obstacle physique. |

#### 👁️ 3. Événements de Rendu Graphique et de Caméra
Liés directement à ce que la caméra du joueur voit ou à la façon dont la carte dessine les objets.

| Nom du Nœud | Condition de Déclenchement | Note d'Optimisation IA |
| :--- | :--- | :--- |
| **OnBecameVisible** | L'objet entre dans le champ de vision d'une caméra. | Permet d'activer un script gourmand uniquement quand on le regarde. |
| **OnBecameInvisible** | L'objet sort complètement de la vue de toutes les caméras. | Permet de couper les calculs visuels pour économiser les FPS de la map. |
| **OnPreCull** / **OnPreRender** | Avant que la caméra ne trie ou ne commence à dessiner la scène. | Réservé aux scripts de rendu avancés de bas niveau. |
| **OnPostRender** / **OnRenderImage** | Juste après le rendu de la caméra. | Utilisé pour appliquer des effets de post-traitement personnalisés. |
| **OnRenderObject** | Après le rendu normal de l'arbre de rendu d'Unity. | Utile pour dessiner des éléments graphiques personnalisés (GL). |
| **OnWillRenderObject** | Appelé une fois pour chaque caméra si l'objet est visible. | Permet de mettre à jour des données de rendu spécifiques par caméra. |

#### 🖱️ 4. Événements d'Interaction Souris / Entrées Standard
Principalement utilisés pour les tests de bureau (Desktop Mode) dans l'éditeur.

* `OnMouseDown` / `OnMouseUp` : Clic ou relâchement du clic sur le Collider de l'objet.
* `OnMouseDrag` : Maintenir le clic enfoncé en déplaçant la souris sur l'objet.
* `OnMouseEnter` / `OnMouseExit` : Survol du curseur de la souris sur la géométrie de l'objet.
* `OnMouseOver` : S'exécute en continu à chaque frame tant que la souris reste posée sur l'objet.
* `OnMouseUpAsButton` : Se déclenche uniquement si le clic a été pressé ET relâché sur le même objet (comportement d'un vrai bouton d'interface).

#### 🧪 5. Événements Spéciaux et Particules
* `OnAnimatorIK` / `OnAnimatorMove` : Permet de modifier ou d'intercepter les mouvements de l'animateur (Cinématique Inverse, Root Motion).
* `OnApplicationFocus` / `OnApplicationPause` / `OnApplicationQuit` : Détecte si le joueur change de fenêtre, met son jeu en arrière-plan ou quitte l'application.
* `OnAudioFilterRead` : Permet de capturer et modifier le flux audio brut d'un `AudioSource`.
* `OnParticleCollision` / `OnParticleSystemStopped` / `OnParticleTrigger` : Déclenché lorsqu'une particule (système Shuriken d'Unity) frappe un objet ou finit son cycle.
* `OnTransformChildrenChanged` / `OnTransformParentChanged` : S'exécute si l'arborescence de l'objet est modifiée dynamiquement en jeu.
* `OnValidate` / `Reset` : Événements exclusifs à l'éditeur Unity pour ranger ou corriger des valeurs de l'inspecteur.
