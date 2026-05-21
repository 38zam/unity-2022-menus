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

---

### 📂 Sous-Menu : Special

Ce sous-menu regroupe les structures de contrôle algorithmiques fondamentales de l'Udon Graph. C'est ici que l'on gère la logique conditionnelle (les choix), les répétitions (les boucles) et les annotations de la grille.

#### 📋 Liste exhaustive des nœuds de contrôle de flux

| Nom du Nœud dans l'Interface | Rôle Logique de la Fonction | Note Critique pour l'IA |
| :--- | :--- | :--- |
| **Branch** | **La condition de base (If / Else).** Dirige le flux d'exécution vers la sortie `True` ou `False` selon l'état d'une variable booléenne. | **Le nœud logique le plus utilisé.** Indispensable pour valider une action (ex: Si le joueur a la clé -> Ouvrir la porte). |
| **Comment** | Génère un encadré textuel statique pour annoter la grille. | Sert uniquement à la documentation visuelle. Ce nœud n'a pas d'entrées/sorties de flux et est ignoré à la compilation. |
| **For** | **Une boucle indexée (For Loop).** Répète l'exécution d'un bloc de nœuds un nombre défini de fois entre un index de début et de fin. | Utilisé pour traiter des listes d'éléments de taille fixe (ex: appliquer une modification à un tableau de 10 objets). |
| **While** | **Une boucle conditionnelle (While Loop).** Exécute une action en boucle continue *tant que* la condition booléenne reste vraie. | **Risque de crash critique :** Si la condition ne passe pas à `False` pendant l'exécution de la boucle, Unity et VRChat plantent instantanément (boucle infinie). |

---

### 📂 Sous-Menu : UdonBehaviour

Ce sous-menu est le pilier central de l'interactivité avancée et du réseau dans VRChat. Il regroupe toutes les méthodes permettant à un script Udon d'interagir avec un autre script situé sur le même objet ou sur un objet tiers de la scène.

#### 📋 Liste exhaustive des nœuds de comportement et réseau

| Nom du Nœud dans l'Interface | Rôle Logique de la Fonction | Note Critique pour l'IA |
| :--- | :--- | :--- |
| **UdonBehaviour.constructor** | Constructeur interne de l'instance du composant. | Pratiquement jamais appelé manuellement dans le graph. |
| **UdonBehaviour.DisableInteractive** | Désactive la capacité du joueur à cliquer (Interagir) sur cet objet en jeu. | Idéal pour verrouiller temporairement un bouton ou une porte. |
| **UdonBehaviour.EnableInteractive** | Réactive la capacité d'interaction physique pour le joueur. | Rend l'objet de nouveau cliquable via le pointeur VR/Souris. |
| **UdonBehaviour.Equals** | Compare si la référence de ce script est identique à une autre. | Test d'égalité de référence standard. |
| **UdonBehaviour.GetHashCode** | Renvoie l'identifiant numérique unique du composant en mémoire. | Utilisé pour l'indexation de bas niveau. |
| **UdonBehaviour.GetProgramVariable** | **Lit la valeur d'une variable** se trouvant à l'intérieur d'un autre script Udon. | **Essentiel.** Permet de lier les systèmes (ex: lire le score d'un joueur géré par un script central). |
| **UdonBehaviour.GetProgramVariableNames** | Extrait un tableau contenant tous les noms de variables publiques du script. | Utile pour l'analyse dynamique des scripts. |
| **UdonBehaviour.GetProgramVariableType** | Récupère le type système (.NET) d'une variable spécifique via son nom. | Permet de sécuriser les conversions de données à la volée. |
| **UdonBehaviour.GetType** | Renvoie le type système du composant UdonBehaviour lui-même. | Analyse de structure standard. |
| **UdonBehaviour.Interact** | Force le déclenchement immédiat de l'événement d'interaction du script. | Permet de simuler par code le clic d'un joueur sur l'objet. |
| **UdonBehaviour.SendCustomEvent** | **Exécute une fonction personnalisée (un événement nommé) locale.** | **Le nœud le plus important.** Déclenche un bloc logique à partir de son nom textuel. |
| **UdonBehaviour.SendCustomNetworkEvent** | **Déclenche une fonction à travers le réseau Internet.** | **Crucial pour la synchro.** Permet de répercuter une action chez tous les joueurs de l'instance (ex: ouvrir une porte pour tout le monde). |
| **UdonBehaviour.SetProgramVariable** | **Injecte/Modifie de force la valeur d'une variable** dans un autre script. | Équivalent du "Get", mais configuré pour l'écriture de données externes. |
| **UdonBehaviour.ToString** | Convertit l'identité textuelle du composant en chaîne de caractères. | Utile pour formater des logs explicites dans la console. |

*(Note : Les mentions d'événements comme OnOwnershipTransferred ou OnPlayerJoined visibles dans cette liste d'API sont des rappels de méthodes héritées de l'infrastructure Udon).*

---

### 📂 Sous-Menu : Type

Ce sous-menu expose les méthodes d'analyse et d'inspection de types issues de la classe de réflexion native `System.Type` du framework .NET. Il permet à une IA de valider la nature, les propriétés et la structure des variables avant d'exécuter un traitement lourd.

#### 📋 Liste exhaustive des nœuds d'analyse de type

| Nom du Nœud dans l'Interface | Rôle Logique de la Fonction | Note Critique pour l'IA |
| :--- | :--- | :--- |
| **Type.Equals** | Évalue si deux types d'objets comparés sont rigoureusement identiques. | Sécurisation des embranchements algorithmiques dynamiques. |
| **Type.GetConstructor** / **GetFields** / **GetMethods** / **GetProperties** | Inspecte et extrait les constructeurs, variables (champs), fonctions ou propriétés d'une classe. | Permet l'analyse et la lecture des structures de données complexes. |
| **Type.GetInterface** / **GetInterfaces** / **GetNestedType(s)** | Récupère les interfaces logiques implémentées ou les classes imbriquées. | Analyse structurelle avancée de l'architecture du code. |
| **Type.GetType** / **GetTypeCode** | Génère l'instance du type ou son code d'énumération système (ex: Int32, String). | Point d'entrée pour identifier de quel type de variable il s'agit. |
| **Type.GetTypeHandle** / **GetTypeFromHandle** | Convertit le type vers ou depuis son pointeur de manipulation système (RuntimeTypeHandle). | Utilisé pour optimiser les comparaisons de bas niveau. |
| **Type.IsArray** | Détermine avec certitude si la variable est un tableau de données (liste `[]`). | **Obligatoire** avant d'alimenter une boucle `For` pour éviter les plantages. |
| **Type.IsAssignableFrom** | Détermine si une instance du type actuel peut recevoir et stocker le type spécifié. | Valide la compatibilité lors de l'héritage d'objets 3D ou logiques. |
| **Type.IsClass** / **IsInterface** / **IsValueType** | Vérifie si la donnée est une classe de référence, un contrat logique ou une structure de valeur brute. | Indique à l'IA comment le moteur gère la donnée en mémoire vive. |
| **Type.IsEnum** | Détecte si le type analysé est une énumération (une liste de choix à puces fixes). | Indispensable pour lire proprement les machines d'états de la map. |
| **Type.IsPointer** / **IsPrimitive** / **IsPublic** | Détecte les variables de type pointeur direct, les données numériques de base (int, float) ou leur visibilité globale. | Filtres d'analyse de sécurité pour le compilateur. |
| **Type.ToString** | Convertit le type en une chaîne de caractères lisible (ex: `"UnityEngine.Transform"`). | Permet d'afficher textuellement la nature d'une erreur dans la console. |

---

### 📂 Sous-Menu : VRC (Racine du SDK VRChat)

Ce sous-menu regroupe l'intégralité des classes et fonctions spécifiques à l'écosystème VRChat. Contrairement aux dossiers précédents issus d'Unity ou du langage C#, ces nœuds servent exclusivement à piloter les fonctionnalités internes du jeu et l'infrastructure réseau des instances.

#### 🗺️ Les 3 branches majeures de l'API VRChat

| Dossier Racine | Rôle Global dans l'Infrastructure | Importance pour le Contextage IA |
| :--- | :--- | :--- |
| **VRC.Core** | Gestion système interne. | Contient les briques fondamentales et les pipelines de données de bas niveau du jeu. Généralement peu manipulé en Udon Graph direct. |
| **VRC.SDK3** | **Composants d'interaction et Joueurs.** | **Le dossier le plus important.** Regroupe la gestion des joueurs (`VRCPlayerApi`), l'accès aux stations (sièges), aux miroirs VRChat, et aux nouveaux outils de conteneurs de données (`VRCDataList`, `VRCDataDictionary`). |
| **VRC.Udon** | Moteur d'exécution Udon. | Gère l'encapsulation logicielle et les ponts de communication réseau génériques propres à la machine virtuelle Udon. |

# 🛠️ Index Général de l'API UnityEngine (Udon Graph)

Ce guide centralise l'intégralité des classes, structures et composants natifs d'Unity exposés dans le SDK VRChat. Contrairement aux menus spécifiques à VRChat, cette bibliothèque gère les fondations du moteur de jeu : la physique 3D, le rendu visuel, les calculs mathématiques, les sons et les systèmes d'animation standard.

---

## 📂 Branche Racine : UnityEngine (Index Alpha-Numérique)

### 🅰️ Section A : Accéléromètre, Android, Animations et Audio

| Nom de la Classe / Namespace | Rôle Logique dans le Moteur | Utilité et Contexte pour l'IA |
| :--- | :--- | :--- |
| **AccelerationEvent** | Capture les données physiques de l'accéléromètre de l'appareil. | Lié aux mouvements physiques (Utile pour des mécaniques mobiles spécifiques). |
| **Android** | Fonctions et passerelles dédiées à la plateforme mobile Android. | **Indispensable pour l'optimisation Quest.** Permet d'isoler du code propre à la version autonome (Meta Quest). |
| **Animation...** (`AnimationClip`, `Curve`, `State`) | Composants du système d'animation historique (Legacy) d'Unity. | Permet de lire des états d'animation simples ou de manipuler des courbes temporelles. |
| **Animator...** (`ClipInfo`, `Parameter`, `StateInfo`) | **Moteur d'animation Mecanim.** Gère les machines à états et les transitions complexes. | **Crucial pour l'interactivité.** Permet à l'IA de modifier les paramètres d'un Animator (ex: ouvrir une porte, lancer une cinématique). |
| **Application** | Accès aux métadonnées globales de l'instance de jeu en cours d'exécution. | Permet de vérifier le statut de la plateforme ou de la connexion locale. |
| **AudioClip** | Représente une ressource ou un fichier audio brut (WAV, MP3, OGG). | Permet de stocker ou d'assigner dynamiquement un son à un haut-parleur par script. |
| **Audio...Filter** (`Distortion`, `Echo`, `LowPass`) | Filtres d'effets acoustiques en temps réel applicables sur les flux sonores. | **Essentiel pour l'ambiance.** Simule des environnements réalistes (ex: écho dans une grotte, filtre passe-bas derrière un mur). |

---

### 🇨 Section C : Cinemachine, Collideurs et Couleurs

| Nom de la Classe / Namespace | Rôle Logique dans le Moteur | Utilité et Contexte pour l'IA |
| :--- | :--- | :--- |
| **Cinemachine...** (`DollyCart`, `Path`, `VirtualCamera`) | Système de caméras intelligentes et de suivis de trajectoires. | **Le top pour les cinématiques.** Permet de piloter des caméras fluides le long d'un rail (`Path`) pour des intros de map ou des modes spectateur. |
| **Cloth** | Gère les simulations physiques de tissus (vêtements, drapeaux). | Permet d'interagir par code avec les contraintes physiques des tissus sur la map. |
| **Collider** / **Collider2D** | Base de toutes les formes physiques de collision 3D / 2D (Box, Sphere, Mesh). | **La base des interactions.** Permet de détecter quand un joueur ou un objet entre dans une zone (`OnTriggerEnter`). |
| **Collision** / **Collision2D** | Contient toutes les données générées lors d'un impact physique (vitesse, points d'impact). | Permet de calculer les dégâts d'un impact ou de faire rebondir des objets de manière réaliste. |
| **Color** / **Color32** | Représentation mathématique des couleurs (RGBA ou valeurs 0-255). | Permet de changer dynamiquement la couleur d'une lumière, d'une interface UI ou d'un matériau. |
| **Component** | Classe mère de tout ce qui s'attache à un objet dans la hiérarchie d'Unity. | Permet de rechercher et manipuler les scripts ou composants reliés à un objet. |
| **Coroutine** | Permet de mettre un script en pause et d'attendre un certain temps avant de continuer. | Gérée de façon spécifique via les événements `SendCustomEventDelayed` dans Udon. |
| **Debug** | Contient les outils d'affichage de logs dans la console de développement. | **Essentiel pour le débuggage.** Permet d'écrire des messages de suivi (`Debug.Log`) visibles dans l'UdonLog ou le Creator Companion. |

---

### 🇩 Section D à F : Moteur Physique, Affichage et Joints

| Nom de la Classe / Namespace | Rôle Logique dans le Moteur | Utilité et Contexte pour l'IA |
| :--- | :--- | :--- |
| **Display** | Gère les moniteurs d'affichage et la gestion multi-écrans. | Utilité limitée en VR, sert principalement pour les configurations d'affichage PC de bas niveau. |
| **DrivenRectTransformTracker** | Verrouille et pilote les dimensions des éléments d'interface UI. | Utilisé de façon interne par Unity pour stabiliser la mise en page automatique des menus. |
| **DynamicGI** | Contrôle l'Illumination Globale dynamique (lumières indirectes en temps réel). | Permet de modifier par code l'intensité des reflets de lumière précalculés sur les surfaces mobiles. |
| **FixedJoint** / **FixedJoint2D** | Joint physique qui attache rigidement deux corps physiques ensemble. | Utile pour lier deux objets (ex: créer un wagon détachable ou assembler des pièces mécaniques). |
| **Flare** / **FlareLayer** | Gère les effets d'éblouissement optique (Lens Flares) provoqués par les sources lumineuses. | Permet d'activer ou désactiver les effets de halo de lumière directement sur la caméra. |
| **Font** | Représente les polices de caractères utilisées pour l'affichage de textes standard. | Permet de changer dynamiquement la typographie d'un panneau d'affichage textuel. |

---

### 🇬 Section G à H : GameObjects, Graphismes et Squelettes

| Nom de la Classe / Namespace | Rôle Logique dans le Moteur | Utilité et Contexte pour l'IA |
| :--- | :--- | :--- |
| **GameObject** | **L'entité fondamentale de base d'Unity.** Tout objet présent sur la scène en est un. | **Nœud ultra-utilisé.** Permet d'activer/désactiver un objet (`SetActive`), de l'instancier ou de détruire un élément du décor. |
| **Gizmos** | Dessine des formes d'aide visuelle directement dans la vue Scène d'Unity. | Permet aux créateurs de visualiser des zones de déclenchement ou des chemins invisibles en jeu. |
| **Gradient** | Gère une transition fluide entre plusieurs couleurs successives. | Idéal pour créer des effets visuels évolutifs, comme une jauge de vie qui passe du vert au rouge. |
| **Graphics** | Commandes graphiques de bas niveau pour dessiner directement des textures ou des meshes. | Utilisé pour optimiser l'affichage ou manipuler des buffers d'effets visuels avancés. |
| **Gyroscope** | Capte les données de rotation spatiale de l'appareil mobile. | Principalement exploité pour adapter des contrôles spécifiques sur les supports mobiles autonomes. |
| **HingeJoint** / **HingeJoint2D** | Joint physique de type "charnière" (axe de rotation pivotant libre). | **Idéal pour la physique.** Permet de concevoir des portes physiques réalistes, des roues ou des leviers à actionner. |
| **HumanBone** / **HumanPose...** | Structures de données décrivant l'armature humanoïde et l'état de sa pose. | Lié à la configuration anatomique des avatars ou des mannequins interactifs dans le monde. |

---

### 🇮 Section I à L : Inputs, Lumières et Calques

| Nom de la Classe / Namespace | Rôle Logique dans le Moteur | Utilité et Contexte pour l'IA |
| :--- | :--- | :--- |
| **Input** / **InputSystem** | Gère la lecture des actions de l'utilisateur (clavier, souris, manettes VR). | Permet de détecter si un joueur appuie sur une touche spécifique pour activer une lampe de poche ou un menu. |
| **Joint** / **JointLimits** | Classes génériques de gestion des liaisons physiques articulées. | Définissent les angles limites et l'élasticité des liaisons de corps physiques (ex: élastiques, cordes). |
| **LayerMask** | Système de filtrage basé sur les calques (Layers) de la scène. | **Crucial pour l'optimisation des lancers de rayons.** Permet d'ignorer certains objets lors d'un calcul de tir laser. |
| **Light** | Contrôle l'intégralité des sources lumineuses (Directionnelle, Point, Spot). | Permet d'allumer/éteindre une lampe, de modifier sa couleur ou d'ajuster l'intensité de l'éclairage de la map. |
| **LightmapSettings** / **LightProbes** | Gère les données de rendu et d'éclairage statique précalculé (Baked Lighting). | Permet de modifier l'ambiance lumineuse générale ou d'appliquer des préréglages de rendu globaux. |

---

### 🇲 Section M à P : Meshes, Matériaux, Mathématiques et Physique

| Nom de la Classe / Namespace | Rôle Logique dans le Moteur | Utilité et Contexte pour l'IA |
| :--- | :--- | :--- |
| **LineRenderer** | Dessine une ligne continue en 3D reliant une série de coordonnées spatiales. | Idéal pour tracer des lasers d'armes, des trajectoires de téléportation ou des lignes de dessin 3D. |
| **LODGroup** | Gère le niveau de détail (Level of Detail) des modèles 3D selon la distance de la caméra. | Crucial pour l'optimisation des performances des gros éléments de décor. |
| **Material** | Définit l'apparence visuelle d'une surface 3D (Couleur, texture, propriétés de brillance). | Permet de changer l'aspect d'un mur ou d'un objet en plein jeu (ex: faire fondre de la glace en changeant son shader). |
| **Mathf** | **Bibliothèque complète de fonctions mathématiques.** (Sinus, Cosinus, Lerp, Clamp...). | **Le moteur des animations par code.** Permet de calculer des trajectoires fluides, de limiter des valeurs ou de créer des oscillations. |
| **Mesh** / **MeshFilter** / **MeshRenderer** | Contient la géométrie 3D brute (les triangles) et s'occupe de l'afficher à l'écran. | Permet de remplacer dynamiquement la forme 3D d'un objet (ex: transformer un cube en sphère par script). |
| **Microphone** | Gère la capture audio des périphériques d'enregistrement locaux. | Soumis aux restrictions de sécurité de VRChat, la voix passant principalement par le système audio VRChat natif. |
| **ParticleSystem** | **Moteur de particules d'Unity (Shuriken).** Gère les effets visuels fluides. | Permet de déclencher par script des feux d'artifice, de la fumée, des explosions ou des effets magiques. |
| **Physics** | **Moteur de simulation physique 3D globale.** (Gravité, lancers de rayons). | **Contient le nœud indispensable `Physics.Raycast`.** Permet de simuler des tirs d'armes, de détecter ce que regarde le joueur ou de mesurer les distances. |
| **PlayerPrefs** | Système de stockage local persistant de données sur la machine de l'utilisateur. | Dans VRChat, son comportement est encadré pour éviter la triche; on lui préfère souvent la persistance native de VRChat. |

---

### 🇶 Section Q à S : Rotations, Écrans et Shaders

| Nom de la Classe / Namespace | Rôle Logique dans le Moteur | Utilité et Contexte pour l'IA |
| :--- | :--- | :--- |
| **Quaternion** | Structure mathématique complexe dédiée au calcul des rotations 3D (sans blocage de cardan). | **Indispensable pour orient
