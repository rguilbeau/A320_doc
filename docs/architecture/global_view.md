# Vue globale

Pour faire fonctionner le cockpit il est obligatoire de récupérer les variables de l'avion dans dans MSFS et de lui envoyer des événements.

Cette vue montre grossièrement l'architecture entre les différentes partie du projet :

![Global](../_assets/architecture/architecture.drawio)

`A320_Cockpit.exe` est l'application qui permet l'échange des données entre MSFS et le cockpit.

- La communication entre MSFS et l'application `A320_Cockpit.exe` se fait via la librairie officel **SimConnect**. Cette libraire créer un zone de transfert de données (_shared memory_)
- Le module **WASM** est un plugins développé pour MSFS pour permettre de récupérer des varibales normalement non accessible directement via **SimConnect**
- La communication entre l'application `A320_Cockpit.exe` et le cockpit se fait via un bus CAN (ce bus est connecté au PC via USB avec le module _CANtact_)
- L'application `A320_Cockpit.exe` récupère les variables MSFS et les transforme en frames CAN pour les envoyer aux différents modules
- Tous les modules du cockpits sont indépendants, chacun d'eux représente un noeud sur le bus CAN, les modules traitent les frames CAN qui leurs sont important



## Variables MSFS

Il existe plusieurs types de varibales MSFS:

- **SimVar:** Les variables "native" à flight simulator
- **LVar:** Les variables spécifiques définit par l'avion

Les variables **SimVar** peuvent être récupéré directement grâce à l'API **SimConnect**, quant aux autres variables elles ne peuvent être accédés que via le plugin **Wasm**.

> **Note:** Même si les variables sont récupéré via le module **Wasm** la communication se fait malgrès tout via **SimConnect**, la zone d'échange n'est pas la même et est définit spécifiquement entre `A320_Cockpit.exe` et le module **Wasm**