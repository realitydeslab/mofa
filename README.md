# MOFA — Multiplayer Omnipresent Fighting Arena


<p align="center">
    <a href="https://arxiv.org/abs/2505.12516"><img alt='arXiv' src="https://img.shields.io/badge/arXiv-2505.12516-b31b1b.svg"></a>
    <a href="https://reality.design/project/mofa"><img alt='page' src="https://img.shields.io/badge/Project-MOFA-orange"></a>
  </p>


**MOFA** is an open‑source mixed‑reality (MR) game framework for **co‑located, bodily, movement‑based play** in public spaces. It ships three exemplar “realities” (game modes) used in the research paper:

- **The Training** — single‑player spell practice  
- **The Duel** — two‑player wizard dueling  
- **The Dragon** — cooperative monster hunting for 3+ players

MOFA emphasizes *omnipresent* play: spontaneous sessions that work **without Wi‑Fi, cellular, or internet**, using peer‑to‑peer networking. Spell‑casting gestures with a **physical wand** keep interactions intuitive and avoid physical contact between players for safety. [oai_citation](https://arxiv.org/html/2505.12516v1)

📄 **Paper**:
If you build on this framework in research or production, please cite:

> Botao Amber Hu, Rem RunGu Lin, Yilan Elan Tao, Samuli Laato, and Yue Li (2025). "Towards Immersive Mixed Reality Street Play: Understanding Co-located Bodily Play with See-through Head-mounted Displays in Public Spaces". In Proceedings of the ACM on Human-Computer Interaction (CSCW 2025). https://doi.org/10.1145/3757679 or preprint https://arxiv.org/abs/2505.12516
 
The paper introduces MOFA and details the design choices above. 

### What’s in this repo

A Unity project that implements MOFA on top of the HoloKit stack, organized like **holokit-app** (package‑based), with each gameplay mode as its own “reality.” You’ll find:

```
Packages/
org.realitydeslab.mofa.librarybase/            # Core: input, spells, hit/shield logic, P2P bootstrap
org.realitydeslab.mofa.the-training/     # ‘The Training’ sample
org.realitydeslab.mofa.the-duel/         # ‘The Duel’ sample
org.realitydeslab.mofa.the-dragon/       # ‘The Dragon’ sample
```

> The package‑oriented layout mirrors [holokit-app](https://github.com/realitydeslab/holokit-app), where each “reality” is a package depending on a common base.  
> 

### Quick start

1. Clone with Git LFS
holokit‑app uses Git LFS for large files.

2.	Open in Unity (LTS)
Open the project with Unity 6 LTS (the version holokit‑app targets). 

3.	Set build target to iOS
Switch Platform → iOS. Ensure your signing team/profiles are set in Player Settings → iOS.

4.	Run a local session
Build to two (or more) iPhones. Launch the same MOFA reality scene on each device and start a session (one device hosts, others join). See Networking.

### Requirements
	•	Unity: 6 LTS recommended.
	•	iOS device: Use an iPhone supported by HoloKit X (see compatibility guide).
	•	Xcode: Use a recent Xcode 26 for iOS 26.
	•	HoloKit Unity SDK: Follow the SDK’s stated OS/ARFoundation requirements. 

MOFA targets optical see‑through MR with HoloKit (iPhone‑powered). If you port to other MR HMDs, update input, render, and transport layers accordingly.  ￼


### Build & run (iOS + HoloKit)
	1.	In Unity, open one of the MOFA sample scenes (e.g., Training, Duel, Dragon).
	2.	Build for iOS and install to devices.
	3.	Launch on all devices; start a session on one device and join from others.
	4.	Put iPhones in HoloKit X headsets; hand out wands; play.

The project uses Apple’s native plugins (CoreHaptics/PHASE) when available to improve immersion; see the “Apple Unity Plugins” guidance in holokit‑app if you plan to compile those native libs yourself.  ￼

###  Gameplay modes
	•	The Training — Solo spell‑casting practice with a wand to learn gestures and timing.
	•	The Duel — Two players face off using ranged “energy” attacks and shields; contact‑free by design to reduce risk in public spaces.
	•	The Dragon — Three or more players cooperate to hunt a shared target.

All modes are designed for street‑safe, public‑space play: explicit targets (shields), no physical contact, and mechanics that read well to bystanders.  ￼


### Architecture

MOFA follows the holokit-app pattern:
	•	Package‑based realities — each game mode is a self‑contained Unity package that depends on a base library (mirroring holokit‑app’s Holoi.Library.HoloKitAppLib).
	•	Common systems — player pose sync, wand input, spell projectiles/shields, UI, and URP configuration live in com.mofa.framework.
	•	Netcode — gameplay logic runs inside Unity’s Netcode for GameObjects context.

This structure makes it straightforward to add your reality: duplicate the template package, rename its package.json and assembly definition, update the Reality ScriptableObject (name, bundle ID, description), then implement your RealityManager that derives from the base manager. See holokit‑app’s “How To Create A New Reality” for the model we follow.  ￼


### Networking

MOFA’s omnipresent collocation uses peer‑to‑peer local networking (no router, no internet) via Apple’s MultipeerConnectivity as a transport under Netcode. This is the same technology behind AirDrop, enabling spontaneous play virtually anywhere.  ￼
	•	Transport: We integrate a Multipeer Connectivity Transport package that plugs into Netcode for GameObjects. Use it when building for iOS/visionOS/macOS to get local P2P sessions.  ￼
	•	Architecture: Netcode’s gameplay layer remains host‑client, while Multipeer provides low‑latency peer discovery/links under the hood.  ￼


### Spectator view

The package‑based, networked architecture supports spectator view—great for capturing and sharing videos of matches. See the rationale in holokit‑app’s docs; MOFA realities follow the same pattern.  ￼


### Safety & field use

MOFA is designed for public‑space play. Please:
	•	Choose open, obstacle‑free areas; avoid traffic and crowds.
	•	Keep contact‑free distance; use the wand and ranged spells only.
	•	Brief participants and bystanders; obtain permissions where required.
	•	Follow local laws and venue policies.

These guidelines reflect the paper’s emphasis on bystander inclusion and safety when bringing MR into streets and public venues.  ￼

### Troubleshooting
	•	Xcode “cycle” build step order
If you see “Cycle inside Unity‑iPhone building…”, re‑order the “Unity Process symbols for Unity‑iPhone” build phase as the last step before “Embed Watch Content,” as noted in holokit‑app.  ￼
	•	Apple Unity Plugins “CopyAndEmbed .framework” error
Apply the fix described in holokit‑app’s notes (linking to the upstream PR).  ￼
	•	Devices don’t see each other
Ensure Bluetooth and Wi‑Fi are enabled (MultipeerConnectivity uses them for discovery), and that all devices run the same build.  ￼


### Contributing

Issues and PRs are welcome! If you’re adding a new game mode, please follow the package structure above. For network features, prefer the Multipeer transport in Apple ecosystems; document alternatives if you target other platforms.


### License

This repository is released under the MIT License (see LICENSE).  


### Notes on provenance (for maintainers)

- **MOFA concept & modes** (“Training”, “Duel”, “Dragon”), wand interactions, safety (contact‑free), and *omnipresent* P2P design are from the [CSCW 2025 paper](https://arxiv.org/html/2505.12516v1)  
- **holokit‑app** provides the package‑based Unity structure, LFS usage, recommended Unity version, native plugin pointers, troubleshooting notes, and “create a new reality” workflow that this README mirrors. [holokit-app](https://github.com/realitydeslab/holokit-app) 
- **HoloKit Unity SDK** compatibility evolves; link users there for current iOS/ARFoundation details.  [holokit-unity-sdk](https://github.com/holokit/holokit-unity-sdk)  
- **MultipeerConnectivity Transport** package is Reality Design Lab’s Netcode transport used for P2P.  [netcode-transport-multipeer-connectivity](https://github.com/realitydeslab/netcode-transport-multipeer-connectivity)

