+++
title = "Veille technologique"
weight = 1
+++

## 1. Premiers prompts et réponse avec un LLM

### Outil utilisé
- **Assistant / Modèle** : Antigravity — Gemini 4 Neon (Medium)
- **Mode actif** : Recherche web activée (`search_web`)
- **Date** : 25 novembre 2026

### Prompt initial
 >Le sujet est l'outil de compatibilité Proton sur linux. Fait une recherche sur le web pour avoir une idée de ce que c'est et comment cela fonctionne en bref.

### Réponse générée

#### Qu'est-ce que Proton ?

**Proton** est une **couche de compatibilité open-source** développée par **Valve** (en collaboration avec CodeWeavers) et intégrée à Steam (Steam Play) ainsi qu'à SteamOS / Steam Deck. Elle permet d'exécuter des jeux et applications conçus pour Windows sur Linux, **sans émulation ni machine virtuelle**.

Au lieu d'émuler un CPU ou Windows au complet, Proton **traduit à la volée** les appels d'API Windows vers leurs équivalents natifs Linux (POSIX, Vulkan, noyau Linux), ce qui permet d'obtenir des performances proches du natif.

#### Comment ça fonctionne en bref ?

```text
[ Jeu / Application Windows ]
            │  (Appels Win32 + Direct3D)
            ▼
   [ Wine (noyau Proton) ] ──► Traduit les appels système Win32 en appels POSIX (Linux)
            │
            ├──► [ DXVK ]          (Direct3D 8/9/10/11 ──► Vulkan)
            └──► [ VKD3D-Proton ]  (Direct3D 12        ──► Vulkan)
                                          │
                                          ▼
                              [ Pilotes GPU Linux ] (Mesa / AMD / NVIDIA)
                                          │
                                          ▼
                                     [ Matériel ]
```

**Les briques principales :**
1. **Wine (la fondation)** : réimplémente les DLL système Windows (`kernel32.dll`, `ntdll.dll`, `user32.dll`, etc.) et traduit les appels système (fichiers, fenêtres, mémoire) en appels POSIX. Proton crée un **préfixe Wine** (`pfx`) isolé par jeu (faux disque `C:`, registre Windows, dépendances Visual C++).
2. **DXVK** : traduit les appels graphiques **DirectX 8, 9, 10 et 11** vers **Vulkan** et compile les shaders HLSL/DXBC en **SPIR-V** (avec un cache de shaders pour limiter les saccades).
3. **VKD3D-Proton** : fork spécialisé par Valve qui traduit **DirectX 12** vers **Vulkan** (incluant des fonctionnalités modernes comme le Ray Tracing / DXR et les Mesh Shaders).
4. **Synchronisation des threads (ESync / FSync / NTSync)** : les primitives de synchronisation Windows ne correspondent pas directement à Linux. Proton a d'abord utilisé **ESync** (`eventfd`) puis **FSync** (`futex2`), et intègre désormais **NTSync** (pilote intégré dans le noyau Linux 6.14+ et dans SteamOS 3.7.20+) qui reproduit nativement la synchronisation NT dans le noyau pour réduire la latence CPU.
5. **Audio, médias et manettes** : traduction audio (FAudio / PulseAudio / PipeWire), décodage vidéo via GStreamer, et gestion unifiée des manettes via Steam Input.
6. **Écosystème & variantes** : en plus du Proton officiel (dépôt `ValveSoftware/Proton`), la communauté maintient **GE-Proton** (GloriousEggroll) qui intègre des correctifs plus récents et des codecs supplémentaires, ainsi que **ProtonDB** pour suivre la compatibilité par jeu.

**Sources mentionnées par le LLM :**
- Dépôt officiel Valve Proton : https://github.com/ValveSoftware/Proton
- Articles techniques & presse Linux : GamingOnLinux, Phoronix, TechPowerUp, Linux Journal, Dev.to, Wikipédia (Proton)
- Communauté : ProtonDB, forums Steam Community, Reddit (`r/linux_gaming`, `r/SteamDeck`)

### Discussion critique de la réponse

- Le LLM a généré une réponse qui explique brièvement et en détail ce que j'ai demandé. Il a même fait un diagramme que je n'ai pas demandé pour mieux expliquer. Il a ajouté aussi les sources utilisés lors de la recherche web. Les informations données me semble bons par rapport à ce que je connais mise à part l'explication des des briques principales. Sinon, pour la description de Proton, c'est très cohérent.
---

## 2. Audit des citations
*(À compléter : ouvrir les sources citées par l'IA et vérifier si le modèle résume fidèlement les articles originaux ou s'il extrapole des faits.)*

---

## 3. Comparaison méthodologique
*(À compléter : comparer cette synthèse avec une recherche manuelle directe — documentation officielle, dépôts GitHub, moteurs de recherche — et/ou un second LLM.)*

---

## 4. Source pivot
*(À compléter : identifier la ressource de référence centrale qui couvre le sujet en profondeur.)*

---

## 5. Corpus pour les notes de cours
*(À compléter : lister les sources primaires retenues — documentations, dépôts, billets d'ingénierie — et justifier leur choix.)*

---

## 6. Sources communautaires et veille continue
*(À compléter : chaînes YouTube techniques, développeurs clés, sites spécialisés, dépôts GitHub/GitLab, flux RSS.)*
