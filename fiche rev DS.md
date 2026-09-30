
# **Chapitre 1 : Évolution des architectures de déploiement et l'OCI**

## **1. Évolution des architectures de déploiement**

### **Comparaison des trois modèles de déploiement (Explication du schéma)**

L'évolution des infrastructures informatiques est représentée à travers trois générations d'architectures[1] :

- **Déploiement Traditionnel (** **Traditional Deployment** **/** **Bare Metal** **)** :
    - **Description de l'empilement** : Les applications sont exécutées directement sur le système d'exploitation de la machine physique[1].
    - **Structure des couches (de bas en haut)** :
        1. **Hardware** (Matériel physique)[1].
        2. **Operating System** (Système d'exploitation hôte)[1].
        3. **App** (Applications installées directement)[1].
- **Déploiement Virtualisé (** **Virtualized Deployment** **)** :
    - **Description de l'empilement** : L'introduction d'un hyperviseur permet de faire fonctionner plusieurs machines virtuelles (VM) étanches sur une même infrastructure physique[1].
    - **Structure des couches (de bas en haut)** :
        1. **Hardware** (Matériel physique)[1].
        2. **Operating System** (Système d'exploitation hôte)[1].
        3. **Hypervisor** (Hyperviseur)[1].
        4. **Virtual Machine** (Chaque VM embarque son propre système d'exploitation **Operating System** [OS invité], ses bibliothèques **Bin/Library**, et ses applications **App**)[1].
- **Déploiement Conteneurisé (** **Container Deployment** **)** :
    - **Description de l'empilement** : L'hyperviseur et les OS invités sont remplacés par un moteur d'exécution de conteneurs (_Container Runtime_), mutualisant le noyau du système d'exploitation hôte[1].
    - **Structure des couches (de bas en haut)** :
        1. **Hardware** (Matériel physique)[1].
        2. **Operating System** (Système d'exploitation hôte)[1].
        3. **Container Runtime** (Moteur d'exécution des conteneurs)[1].
        4. **Container** (Chaque conteneur regroupe uniquement les dépendances **Bin/Library** et les applications **App**)[1].

---

## **2. L'Open Container Initiative (OCI)**

### **Définition exacte et caractéristiques**

- **Définition exacte** : L'**Open Container Initiative (OCI)** est une structure de gouvernance ouverte créee dans le but exprès de créer des normes industrielles ouvertes autour des formats et des durées d'exécution (_runtimes_) des conteneurs[1].
- **Année de création** : **2015**[1].
- **Langage principal** : **Go**[1].
- **Écosystème et gouvernance** : Associée à des projets ouverts hébergés sur **Github** et liée à la **Fondation Apache**[1].
- **Runtime de référence OCI** : **runC** (Container runtime bas niveau)[1].
- **Lien du dépôt binaire/code** :


https://github.com/opencontainers/runc

---
## 3. Le Container Runtime (Moteur d'exécution)

### Définition exacte
* **Définition** : Dans le monde des conteneurs, la **runtime** est responsable de **créer** et de **faire fonctionner** le conteneur [1].

### Cycle de vie et commandes/opérations du Runtime (Explication du schéma)
Le runtime interagit directement avec le conteneur pour piloter son exécution selon **5 opérations fondamentales** [1] :


1. Create     (Création du conteneur)
2. Start      (Démarrage du conteneur)
3. Get state  (Obtention de l'état actuel)
4. Kill       (Arrêt / Interruption du conteneur)
5. Delete     (Suppression du conteneur)


### Structure interne d'un conteneur et portabilité (Explication du schéma)
* **Composants internes du conteneur** :
  * **Application Code** : Code source ou binaire applicatif [1].
  * **Dependencies** : Paquets applicatifs, bibliothèques (*libs*), utilitaires système (*OS utils*) [1].
  * **Processus** : Un ou plusieurs processus exécutés en isolation (`process 1`, ..., `process N`) [1].
* **Portabilité multi-environnements** :
  * Les conteneurs garantissent un fonctionnement identique ("*Containers work the same way on*") sur [1] :
    * **Laptop** (Ordinateur portable / poste de développement) [1].
    * **Server** (Serveur physique / centre de données) [1].
    * **Cloud** (Fournisseurs de services Cloud) [1].

---

## 4. Concepts clés et notions fondamentales

* **Immuabilité** :
  * **Définition** : L'application doit fonctionner de la même manière et avoir exactement le même comportement, depuis le poste de développement jusqu'au serveur en production [2].
* **Augmentation de la densité** :
  * **Principe** : Les conteneurs ayant un impact bien plus faible que les machines virtuelles (VM) sur les hôtes (*hosts*), ils permettent d'optimiser l'utilisation des ressources matérielles et d'augmenter la densité d'applications par serveur [2].

---

💡 **Souhaitez-vous passer dès maintenant aux notes de cours du Chapitre 2 (consacré aux mécanismes du noyau Linux comme les Namespaces, Cgroups, SELinux et Seccomp) ?**






# Chapitre 2 : Architecture interne du noyau Linux pour l'isolation

## 1. Vue d'ensemble de l'architecture Linux pour les conteneurs

### Rôle du noyau Linux

Le fonctionnement des conteneurs repose sur l'utilisation directe de fonctionnalités natives du noyau Linux[1][2]. L'interaction entre les conteneurs et les composants du noyau s'organise selon plusieurs couches d'isolation, de gestion de ressources et de sécurité[1][2].

### Schéma explicatif : Empilement des couches du noyau (Explication du schéma)

L'architecture globale assurant l'isolation et la sécurité d'un conteneur repose sur la superposition des composants suivants (de la base matérielle vers les applications)[2] :

1. **Infrastructure** : Support matériel physique, virtualisé ou cloud (_Physical / Virtual / Cloud_)[2].
2. **Espaces de noms (** **Namespaces** **)** : Couche d'isolation assurant l'abstraction des ressources système globales[1][2].
3. **Groupes de contrôle (** **Cgroups** **)** : Couche de gestion matérielle contrôlant spécifiquement[2][3] :

- **CPU**[2][3]
- **RAM** (Mémoire système)[2][3]
- **STORAGE** (I/O disque)[2][3]
- **NETWORK** (Bande passante réseau)[2][3]

4. **Règles Seccomp (** **Seccomp Rules** **)** : Couche de filtrage des appels système (_syscalls_)[2].
5. **SELinux** : Couche supérieure de contrôle d'accès obligatoire (_MAC_)[2][4].
6. **Conteneurs (** **Containers** **)** : Applications exécutées avec leurs dépendances (_Libs/Bins_) au-dessus des règles de sécurité[2].

---

## 2. Les Espaces de Noms (_Namespaces_)

### Définition exacte

- **Définition** : Le noyau assure l'isolation des processus en créant des espaces de noms distincts pour les conteneurs[1]. Les espaces de noms permettent de créer une abstraction d'une ressource système globale particulière et de la faire apparaître comme une instance séparée pour les processus au sein d'un espace de noms[1]. Par conséquent, plusieurs conteneurs peuvent utiliser la même ressource simultanément sans créer de conflit[1].

### Typologie des espaces de noms

Les différents types d'espaces de noms couvrent chacun une ressource système spécifique[1] :

- **Les espaces de noms de montage (** **Mount namespaces** **)** :
    - **Fonction** : Isolent l'ensemble des points de montage du système de fichiers vus par un groupe de processus[5].
    - **Effet** : Les processus situés dans différents espaces de noms de montage peuvent avoir différentes vues de la hiérarchie du système de fichiers[5].
- **Les espaces de noms UTS (** **UTS namespaces** **)** :
    - **Fonction** : Isolent deux identifiants système renvoyés par l'appel système `uname()`[5] :
        1. **Nodename** (Nom d'hôte)[5].
        2. **Domainname** (Nom de domaine NIS)[5].
    - **Apport** : Permet à chaque conteneur d'avoir son propre nom d'hôte et son propre nom de domaine NIS, ce qui est utile pour les scripts d'initialisation et de configuration basés sur ces noms[5].
    - **Syntaxe de l'appel système associé** :

```
uname()
```

- **Les espaces de noms IPC (** **IPC namespaces** **)** :
    - **Fonction** : Isolent certaines ressources de communication interprocessus (_IPC - Inter-Process Communication_)[6].
    - **Effet** : Deux conteneurs peuvent créer des segments de mémoire partagée et des sémaphores portant le même nom, sans pouvoir interagir avec les segments de mémoire ou la mémoire partagée d'autres conteneurs[6].
- **Les espaces de noms PID (** **PID namespaces** **)** :
    - **Fonction** : Permettent aux processus de différents conteneurs d'avoir le même **PID** (Identifiant de processus)[6].
    - **Effet** : Chaque conteneur peut posséder son propre processus `init` (**PID 1**) gérant les tâches d'initialisation du système et le cycle de vie des conteneurs[6].

---

## 3. Les Groupes de Contrôle (_Cgroups_)

### Définition exacte

- **Définition** : Le noyau utilise des **cgroups** pour regrouper les processus dans le but de gérer les ressources système[3].

### Ressources allouées par les Cgroups

Les cgroups permettent d'allouer les ressources suivantes ou une combinaison de celles-ci[3] :

- Des ressources **CPU**[3].
- De la **mémoire système** (RAM)[3].
- De la **bande passante réseau**[3].
- L'**accès disque I/O**[3].

### Rôle et bénéfices pour les administrateurs système

Grâce aux cgroups, les administrateurs système disposent d'un contrôle précis pour[3] :

- **Allocation** des ressources[3].
- **Hiérarchisation**[3].
- **Refus**[3].
- **Gestion et surveillance** des ressources système[3].
- **Résultat** : Les ressources matérielles peuvent être intelligemment réparties entre les applications et les utilisateurs, ce qui augmente l'efficacité globale[3].

---

## 4. Sécurisation avancée : SELinux et Seccomp

### Nécessité d'une sécurité complémentaire au niveau du noyau

Bien que les cgroups et les espaces de noms fournissent un premier niveau de sécurité, ils ne suffisent pas à empêcher un processus hostile de sortir d'un conteneur, car l'intégralité du système hôte n'est pas conteneurisée ou isolée dans un espace de noms[4]. Un niveau de séparation supplémentaire est donc indispensable[4].

### Security-Enhanced Linux (SELinux)

- **Définition** : **Security-Enhanced Linux (SELinux)** est une implémentation au niveau du noyau Linux combinant plusieurs mécanismes de sécurité[4] :
    1. **MAC** (_Mandatory Access Control_ / Contrôle d'accès obligatoire)[4].
    2. **MLS** (_Multi-Level Security_ / Sécurité à plusieurs niveaux)[4].
    3. **MCS** (_Multi-Category Security_ / Sécurité à plusieurs catégories)[4].

### Seccomp (Secure Computing)

- **Définition** : **Seccomp** signifie **informatique sécurisée** (_Secure Computing_)[2]. Il s'agit d'une fonctionnalité du noyau Linux utilisée pour restreindre l'ensemble des appels système (_syscalls_) qu'une application ou un conteneur est autorisé à exécuter[2].
- **Chiffres et profil par défaut (Exemple Docker)** :
    - Nombre total d'appels système disponibles dans le noyau Linux : **plus de 300**[2].
    - Nombre d'appels système désactivés par le profil Seccomp par défaut de Docker : **environ 44 appels système**[2].
- **Exemples concrets de restrictions Seccomp cités dans les slides**[2] :
    - **Désactivation de la gestion des modules noyaux** (interdiction de charger/décharger des modules du noyau)[2].
    - **Désactivation de la gestion de l'horloge système** (interdiction de modifier l'heure du système hôte)[2].
- **Exemple de structure de filtre / règles Seccomp** :

```
{
  "defaultAction": "SCMP_ACT_ERRNO",
  "architectures": [
    "SCMP_ARCH_X86_64"
  ],
  "syscalls": [
    {
      "names": [
        "delete_module",
        "init_module",
        "finit_module",
        "clock_settime"
      ],
      "action": "SCMP_ACT_ERRNO"
    }
  ]
}
```




# Chapitre 3 : Runtimes haut niveau (_Docker_, _Podman_, pile logicielle)

1. Distinction entre Runtimes Bas Niveau et Haut Niveau

- **Runtimes bas niveau (****Low-level container runtimes****)** :
    - Ils gèrent les mécanismes de base du noyau Linux (_Namespaces_ et _Cgroups_) pour créer et faire fonctionner le conteneur1.
- **Runtimes haut niveau (****High-level container runtimes****)** :
    - **Définition** : Ils fournissent des couches d'abstraction supplémentaires au-dessus des runtimes bas niveau afin de simplifier l'utilisation et la gestion globale des conteneurs2.
    - **Fonctionnalités fournies** :
        - Des **API**2.
        - Des **interfaces utilisateurs**2.
        - La **gestion des images** (opérations de _pull_ / _push_)2.
        - La gestion du cycle de vie des conteneurs et le dépaquetage des images dans un bundle conforme à l'OCI (_OCI runtime bundle_)3.

---

2. Docker : Historique et Stack Logicielle

Historique et origines de Docker

- **Origine académique** : Projet de fin d'études réalisé par des étudiants de l'école **Epitech** (_Epitech Experience 2006_)2.
- **Fondateurs** : Fondé par **Kamel Founadi**, **Solomon Hykes** et **Sebastien Pahl**2.
- **Évolution de l'entreprise** :
    - Initialement constituée sous forme de **SARL en France** entre **2008 et 2010**2.
    - Faute de pouvoir lever des fonds en France, les fondateurs se sont installés aux **États-Unis** où ils ont bénéficié d'une levée de fonds de **800 000 $** (800k$)2.
- **Année de release officielle de Docker** : **2013**3.

Description de la pile logicielle Docker (_Docker Stack_) (Explication du schéma)

L'architecture logicielle de Docker s'organise en une suite de composants superposés3 :

```
User / CLI
   │
   ▼ (Socket / API)
Docker Client
   │
   ▼ (gRPC Calls)
Docker Daemon
   │
   ▼
containerd (High level container runtime)
   │
   ▼
containerd-shim (Passes container image bundle)
   │
   ▼
runC (OCI container runtime spec reference implementation)
   │
   ▼
Application
```

Rôle détaillé de chaque composant de la pile Docker3 :

- **Docker Client** : Interface en ligne de commande communiquant avec le démon via un Socket ou une API3.
- **Docker Daemon** : Démon central qui reçoit les requêtes et communique avec le runtime haut niveau via des appels **gRPC**3.
- **containerd** :
    - Runtime de conteneur haut niveau (_High level container runtime_)3.
    - Responsable de la **gestion du cycle de vie des conteneurs** (_container lifecycle management_)3.
    - Responsable de la **gestion des images** (_container image management_)3.
    - Dépaquète l'image dans un bundle de runtime OCI (_unpacks the image into an OCI runtime bundle_) et sollicite `runC` pour l'exécuter3.
- **containerd-shim** :
    - Composant intermédiaire transmettant le bundle d'image de conteneur (_Passes container image bundle_)3.
    - Permet au conteneur de rester en cours d'exécution indépendamment de l'état du démon3.
- **runC** :
    - Implémentation de référence de la spécification de runtime OCI (_OCI container runtime spec reference implementation_)3.
    - Code source du runtime bas niveau qui crée effectivement les conteneurs3.

---

3. Podman (_Pod Manager_)

Origine et caractéristiques principales

- **Signification du nom** : **Pod Manager**3.
- **Créateur** : Développé par **RedHat**3.
- **Première release** : **2018** (contre 2013 pour Docker)3.
- **Architecture Daemonless (Sans démon central)** :
    - Il n'y a plus de démon (_daemon_) permanent tournant en arrière-plan3.
    - Tous les conteneurs sont lancés directement par `runC` sans dépendre d'un processus unique3.
- **Compatibilité complète avec Docker** :
    - La compatibilité permet de créer un alias afin de ne pas changer ses habitudes ni ses scripts3 :

```
alias docker=podman
```

- **Intégration Kubernetes** : Podman a été conçu dès l'origine pour fonctionner de pair avec **Kubernetes**3.

---

4. Comparaison des Architectures : Docker vs. Podman (Explication du schéma)

Le support de cours compare directement la chaîne d'exécution entre Docker et Podman3 :

Chaîne d'exécution Docker :

```
Docker CLI ──> Docker Engine ──> Containerd ──> runC ──> Namespaces & Cgroups
```

- Nécessite le passage par le démon central (_Docker Engine_) et le runtime haut niveau (_Containerd_) avant d'atteindre `runC`3.

Chaîne d'exécution Podman :

```
Podman ──> runC ──> Namespaces & Cgroups
```

- Exécution directe de `Podman` vers `runC`, éliminant le besoin d'un démon intermédiaire3.

---

5. Synthèse des Bénéfices Globaux de la Conteneurisation

Les slides synthétisent les avantages majeurs apportés par la conteneurisation3 :

1. **Création et déploiement agile d'applications** : Augmente la simplicité et l'efficacité de la création d'images par rapport à l'utilisation d'images de VM3.
2. **Développement, intégration et déploiement continus (CI/CD)** : Fournit un processus pour construire et déployer fréquemment et de façon fiable avec la capacité de faire des retours en arrière (_rollbacks_) rapides et simples grâce à l'immuabilité de l'image3.
3. **Séparation des besoins entre Dev et Ops** : Création d'images applicatives au moment du _build_ plutôt qu'au déploiement, tout en séparant l'application de l'infrastructure3.
4. **Observabilité** : Informations venant du système d'exploitation sous-jacent et des signaux propres de l'application3.
5. **Consistance entre les environnements** : Fonctionne de la même manière sur un poste local, chez un hébergeur ou dans le Cloud (dev, test, prod)3.
6. **Portabilité entre Cloud et distribution système** : Fonctionne sur Ubuntu, RHEL, CoreOS, _on-premise_, Google Kubernetes Engine (GKE), et n'importe où3.
7. **Gestion centrée Application** : Bascule le niveau d'abstraction d'une virtualisation hardware liée à l'OS à une logique de ressources orientée application3.
8. **Micro-services faiblement couplés, distribués et élastiques** : Les applications sont séparées en petits morceaux indépendants pouvant être déployés et gérés dynamiquement (plutôt qu'une stack monolithique dans une seule machine)3.
9. **Isolation des ressources** : Performances de l'application prédictibles3.
10. **Utilisation des ressources** : Haute efficacité et forte densité3.



# Chapitres 4 & 5 : Nécessité de l'Orchestration, Terminologie et Approches Impératif vs Déclaratif

## Chapitre 4 : Nécessité de l'Orchestration et Terminologie

### 1. Limites de la conteneurisation autonome : Pourquoi l'orchestration ?

L'utilisation de conteneurs isolés montre rapidement ses limites lors du passage à l'échelle ou en environnement de production[1]. Plusieurs problématiques fondamentales nécessitent l'intervention d'un orchestrateur :

- **Gestion de la persistance des données** : Comment assurer la conservation des données au-delà de la durée de vie éphémère d'un conteneur[1] ?
- **Assemblage multi-services** : Comment lier et faire communiquer des services composés de différents conteneurs[1] ?
- **Déploiement multi-nœuds** : Comment déployer et répartir des conteneurs sur plusieurs machines ou serveurs[1] ?
- **Auto-réparation (** **Self-healing** **)** : Comment implémenter des services capables de détecter automatiquement les pannes et de se régénérer[1] ?
- **Débogage en production** : Comment diagnostiquer et débugger des services sur des clusters de production[1] ?
- **Garantie de la qualité de service (QoS)** : Comment garantir la continuité de service en cas de mise à jour ou de panne d'un serveur[1] ?

---

### 2. Terminologie essentielle

- **Cluster** : Ensemble de machines permettant de délivrer, maintenir et mettre à disposition un ensemble de ressources[1][2].
- **Registry** : Bibliothèque ou registre centralisé d'images de conteneurs Docker[2].
    - **Docker Hub** : Registre public hébergeant des images génériques[2].
    - **Harbor** : Registre privé/local (utilisé notamment à l'IUT), permettant d'héberger ses propres versions d'images en toute sécurité sans risque de modifications externes[2].
- **Containers** : Unités d'exécution d'applications[2].
- **Runtimes** : Moteurs d'exécution sous-jacents gérant les conteneurs[2].

---

## Chapitre 5 : Modèles de gestion : Impératif vs Déclaratif

Le déploiement et la gestion d'infrastructures s'articulent autour de deux styles et approches fondamentaux[2] :

### **1. L'Approche Impérative**

- **Définition** : L'approche impérative est une série d'instructions détaillées permettant d'atteindre un état donné (par exemple la commande `apt`)[2].
- **Caractéristiques clés** :
    - On maîtrise l'installation éléments par éléments[2].
    - On configure éléments par éléments[2].
    - On ajuste au fil de l'eau afin d'obtenir le résultat désiré[2].
- **Avantages et limites** :
    - _Avantage apparent_ : Semble plus simple au départ car on dispose de la liste des tâches à effectuer[3].
    - _Limite majeure_ : Ne tolère pas l'interruption ; si la procédure est interrompue, il est souvent nécessaire de reprendre depuis le début[3].

---

### 2. L'Approche Déclarative

- **Définition** : Assimilable à une approche descriptive, elle consiste à décrire un état final désiré. C'est ensuite le système qui s'assure que l'état désiré est bien atteint[4].
- **Caractéristiques clés** :
    - Repose sur un simple fichier dans lequel on décrit ce qu'on souhaite créer, déployer ou maintenir[4].
    - Exige d'avoir défini au préalable chaque élément et paramètre de la configuration[3].
    - **Observation permanente** : Nécessite d'observer l'état en permanence afin de pouvoir intervenir et corriger les dérives[3].
- **Bénéfices de l'approche déclarative** :
    - **Auto-réparation (** **Self-healing** **)**[4].
    - **Maintien en conditions opérationnelles**[4].
    - **Simplicité de gestion**[4].

---

### 3. Illustration par l'Analogie de la Préparation du Thé

Le cours utilise l'analogie de la préparation d'une tasse de thé pour expliciter ces deux paradigmes[4] :

#### Approche Déclarative :

- **Instruction** : « _Je veux une tasse de thé_ »[4].
- **Recette / Description** : Infusion de thé (laisser macérer une infusion dans de l'eau chaude)[4].
- **Analyse de l'approche** :
    - L'approche déclarative nécessite d'avoir défini au préalable chaque élément (durée pour chauffer l'eau, durée d'infusion, quantité, volume de la tasse)[3].
    - _Exemples de paramètres précis cités_[3] :
        - **Thé vert** : Température de 75 à 170 °F, infusé pendant 2 à 3 minutes[3].
        - **Thé noir** : Température de 85 à 185 °F, infusé pendant 4 à 5 minutes[3].
    - Le système surveille l'état en permanence pour garantir le résultat attendu[3].

#### Approche Impérative :

- **Séquence d'instructions pas à pas**[4] :
    1. Faire chauffer de l'eau[4].
    2. Mettre l'eau dans une théière[4].
    3. Ajouter les feuilles de thé[4].
    4. Attendre que cela infuse[4].
    5. Servir dans un verre[4].
- **Analyse de l'approche** :
    - Nécessite de détailler chaque étape manuellement, tout en laissant des incertitudes si la température ou le volume du verre ne sont pas spécifiés[3].
    - En cas d'interruption, la procédure doit généralement être reprise depuis le début[3].

# Chapitres 6 &amp; 7 : Présentation de Kubernetes et Composants du Control Plane

## Chapitre 6 : Présentation de la plateforme Kubernetes

### 1\. Définition et origine de Kubernetes

* **Définition exacte** : **Kubernetes** (communément appelé « **k8s** ») est un système open source qui vise à fournir une plate-forme permettant d'automatiser[1] :
  * La **mise en œuvre** de conteneurs d'application[1].
  * Le **déploiement**[1].
  * La **montée en charge** (*scaling*) sur des grappes de serveurs (*clusters*)[1].
* **Compatibilité** : Il fonctionne avec toute une série de technologies de conteneurisation et est souvent utilisé avec Docker[1].
* **Origine** : Conçu à l'origine par **Google** (issu des projets internes Borg/Omega), puis offert à la **CNCF** (*Cloud Native Computing Foundation*)[1].
* **Rôle principal** : Il s'agit d'un **Orchestrateur de conteneurs**[1].

### 2\. Le positionnement de la plateforme Kubernetes

Kubernetes possède un certain nombre de fonctionnalités. Il peut être considéré comme[1] :

* Une **plate-forme de conteneurs**[4].
* Une **plate-forme de microservices**[4].
* Une **plate-forme cloud portable** et beaucoup plus[4].

#### Caractéristiques de l'environnement de gestion :

* Kubernetes fournit un environnement de gestion focalisé sur le conteneur (**container-centric**)[4].
* Il orchestre les ressources machines (**computing**), la mise en réseau (**networking**) et l'infrastructure de stockage (**storage**) sur les charges de travail (**workloads**) des utilisateurs[4].
* **Positionnement intermédiaire (PaaS vs IaaS)** : Cela permet de se rapprocher de la simplicité des **PaaS** (*Platform as a Service*) avec la flexibilité des solutions **IaaS** (*Infrastructure as a Service*), tout en gardant une portabilité totale entre les différents fournisseurs d'infrastructures (*cloud providers*)[4].

### 3\. Infrastructure en tant que Code (*Infrastructure as Code - IaC*)

* **Définition exacte** : L'**Infrastructure as Code (IaC)** est un ensemble de mécanismes permettant de gérer, par des fichiers descripteurs (généralement au format YAML ou JSON), une infrastructure informatique complète[2].

---

## Chapitre 7 : Les composants du Plan de Contrôle (*Control Plane*)

### 1\. Présentation globale et rôle du Control Plane

* **Définition** : Le **Control Plane** (plan de contrôle) est le **« cerveau » de Kubernetes**[2].
* **Architecture globale (Explication du schéma)**[2] :
  * Le **Control Plane** centralise la logique du cluster[2].
  * Les **Nodes** (ou *Worker Nodes*) sont les machines d'exécution chargées de faire tourner les conteneurs[5].
  * Chaque Node embarque un agent nommé **Kubelet**, responsable de communiquer avec le Control Plane[5].

---

### 2\. L'API Server (`kube-apiserver`)

#### Rôle et fonctionnement :

* **Définition** : L'API Server est une **API REST** chargée de lire et d'écrire dans la base **etcd** en mode **CRUD** (*Create, Read, Update, Delete*), et de notifier ses clients[5].
* **Rôle de pivot central** : Tous les composants du Control Plane communiquent au travers de l'API Server. De plus, l'API Server est l'**unique composant du cluster qui communique directement avec etcd**[5][6].

#### Schéma explicatif : Flux d'une requête dans l'API Server (Explication du schéma)

Lorsqu'une requête HTTP (ex. `HTTP POST`) parvient à l'API Server, elle traverse la séquence de validation suivante avant d'être persistée[7] :

1. **Authentication (Authentification)** : L'API Server vérifie l'identité du client via une chaîne de plugins d'authentification (*Authentication plugin 1, 2, 3...*)[7].
2. **Authorization (Autorisation)** : L'API Server vérifie, grâce à des plugins d'autorisation (ex. RBAC), si le client authentifié dispose des droits pour effectuer l'action demandée[7].
3. **Admission Control (Contrôle d'admission)** : Des plugins d'admission interceptent et peuvent modifier la requête (pour initialiser des champs manquants, appliquer des quotas ou vérifier les politiques de sécurité)[7].
4. **Validation de la ressource** : Contrôle de la conformité du schéma de la ressource[7].
5. **Persistance** : Écriture de l'état validé dans **etcd**[7].

#### Structure d'un fichier YAML pour dialoguer avec l'API Server

Les échanges avec l'API Server s'effectuent via des fichiers descripteurs structurés[8] :

```
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
  labels:
    app: example
spec:
  containers:
    - name: example-container
      image: example/image

```

#### Détail des champs fondamentaux du descripteur YAML :

* **apiVersion** : Indique la version de l'API Kubernetes utilisée pour créer l'objet. La version dépend du type d'objet (ex: `v1` pour un *Pod*, `apps/v1` pour un *Deployment*)[8].
* **kind** : Spécifie le type d'objet créé (*Pod*, *Service*, *Volume*, *Namespace*, *Deployment*, etc.)[8].
* **metadata** : Section servant à décrire et identifier l'objet[9] :
  * **name** : Nom unique de l'objet[9].
  * **labels** : Paires clé-valeur attachées aux objets pour les organiser, les filtrer et les sélectionner[9].
  * **annotations** : Informations complémentaires non identifiantes[9].
* **spec** : Contient les détails spécifiques de l'état désiré pour l'objet (ex. la liste des conteneurs à exécuter, les images, les ports, les volumes à monter)[9].

#### L'outil en ligne de commande : `kubectl` et la configuration `kubeconfig`

* **Définition** : `kubectl` est l'outil CLI officiel pour dialoguer avec l'API Server d'un cluster Kubernetes[6].
* **Fichier de configuration** : Par défaut, `kubectl` recherche un fichier nommé `config` situé dans le répertoire `$HOME/.kube/config`[6].
* **Moyens d'indiquer un autre fichier kubeconfig**[6] :
  1. En définissant la variable d'environnement `KUBECONFIG`[6].
  2. En utilisant l'option `--kubeconfig` en ligne de commande[6].
* **Exemple de commande avec kubeconfig explicite**[6] :

```
kubectl --kubeconfig config apply -f mon-fichier.yaml

```

---

### 3\. Le magasin de données : etcd et le consensus Raft

#### Rôle d'etcd :

* **Définition** : **etcd** est un magasin de données **clé-valeur distribué** qui contient toutes les informations persistantes et l'état du cluster Kubernetes[3][6].
* **Caractéristiques** :
  * Fournit une API simple et offre une **forte cohérence des données** (*strong consistency*)[3][10].
  * Haute disponibilité : possibilité d'exécuter un cluster d'instances etcd[3][6].
  * *Note importante sur les performances* : L'ajout de nœuds dans etcd n'offre pas de meilleures performances (et peut potentiellement les réduire en augmentant le coût de synchronisation), mais offre une **plus grande résilience aux pannes**[3][10].

#### Le protocole de consensus Raft :

* **Principe** : etcd utilise l'algorithme de consensus **Raft** pour déterminer la source de vérité et maintenir la cohérence des données entre les nœuds[3][10].
* **Rôles des nœuds dans un cluster Raft**[11] :
  1. **Leader (Master)** : Nœud central traitant toutes les écritures et répliquant l'état vers les autres nœuds[11].
  2. **Follower** : Nœud récepteur qui applique les réplications du Leader[11].
  3. **Candidate (Candidat)** : État de transition temporaire pris par un Follower lorsqu'il ne reçoit plus de signal (*heartbeat*) du Leader et déclenche une nouvelle élection[11].

---

### 4\. Le Scheduler (`kube-scheduler`)

* **Définition** : Le Scheduler est un client de l'API Server dont le rôle principal est d'**assigner un Node à chaque Pod nouvellement créé ou non lié**[12].
* **Mode d'action** : Il surveille l'API Server pour détecter les Pods en attente d'assignation (`watch`), sélectionne le meilleur nœud d'exécution, puis effectue une requête d'écriture (`bind`) auprès de l'API Server pour lier le Pod à ce nœud[12][13].
* **Facteurs et critères pris en compte lors de la planification (** **Scheduling** **)**[12] :
  * Les exigences individuelles et collectives en ressources (demandes CPU/RAM)[12].
  * Les contraintes matérielles, logicielles et politiques[12].
  * Les spécifications d'**affinité** et d'**anti-affinité** (*nodeAffinity*, *podAffinity* / *podAntiAffinity*)[12].
  * La localité des données et la présence de contraintes de taints/tolerations[12].

---

### 5\. Le Controller Manager (`kube-controller-manager`)

* **Définition** : Démon central fonctionnant comme une **boucle de contrôle continuous** (*control loop*)[14].
* **Principe de fonctionnement** : Il surveille en permanence l'état actuel du cluster via des requêtes vers l'API Server et effectue les ajustements nécessaires pour rapprocher l'état actuel de l'**état souhaité** (*desired state*)[14].
* **Indirection** : Le Controller Manager ne modifie jamais directement les nœuds ou conteneurs ; il passe exclusivement par des requêtes à l'API Server[15].
* **Principaux contrôleurs embarqués**[14] :
  * **Deployment controller** : Gestion du déploiement et des mises à jour applicatives[14].
  * **StatefulSet controller** : Gestion des applications avec état et identité réseau stable[14].
  * **Node controller** : Surveillance de la santé des nœuds du cluster[14].
  * **Service controller** : Gestion des points d'accès réseau[14].
  * **Endpoints controller** : Association des Services aux Pods[14].
  * **Namespace controller** : Gestion de la création/suppression des espaces de nommage virtuels[14].
  * **PersistentVolume controller** : Gestion de l'allocation des volumes de stockage persistant[14].

---

### 6\. Le Kubelet (Agent sur les Worker Nodes)

* **Définition** : Le **Kubelet** est le composant d'exécution principal présent sur chaque **worker node** du cluster[15].
* **Rôle et fonctionnement**[15][16] :
  * Il écoute l'API Server pour identifier les Pods qui ont été assignés à son nœud[15].
  * Il communique avec le runtime de conteneurs local (ex. Docker, containerd) pour démarrer, stopper ou gérer les conteneurs[16].
  * Il surveille l'état des conteneurs, collecte leurs statuts, leurs événements et les ressources consommées, puis remonte ces informations à l'API Server[16].

---

### 7\. Le Cloud Controller Manager (`cloud-controller-manager`)

* **Définition** : Composant facultatif permettant d'isoler les boucles de contrôle spécifiques aux fournisseurs Cloud (*Cloud Providers* comme AWS, GCP, Azure, OpenStack)[16].
* **Avantage** : Permet au code du fournisseur Cloud et au code cœur de Kubernetes d'évoluer indépendamment[16].
* **Contrôleurs dépendants du Cloud**[17] :
  * **Node Controller** : Vérifie auprès de l'infrastructure Cloud si un nœud qui ne répond plus a effectivement été supprimé dans le Cloud[17].
  * **Route Controller** : Configure le routage réseau au niveau de l'infrastructure physique ou virtuelle du Cloud Provider[17].
  * **Service Controller** : Crée, met à jour et supprime les équilibreurs de charge (*Load Balancers*) managés du Cloud[17].
  * **Volume Controller** : Orchestre la création, l'attachement et le montage des disques virtuels fournies par le Cloud Provider[13][17].

---

### 8\. Explication du schéma : Flux typique de création d'un Pod (Sequence Diagram)

Le support de cours illustre le cycle complet de création d'un Pod à travers un flux d'interactions entre l'Utilisateur, l'API Server, etcd, le Scheduler, Kubelet et le Container Runtime (ex. Docker)[13] :

```
Utilisateur / kubectl        API Server          etcd          Scheduler        Kubelet        Docker
       │                          │               │                │               │              │
       │─── 1. create Pod ───────&gt;│               │                │               │              │
       │                          │─── write ────&gt;│                │               │              │
       │                          │&lt;── ack ───────│                │               │              │
       │&lt;── response OK ──────────│               │                │               │              │
       │                          │               │                │               │              │
       │                          │&lt;──────────────┼── watch(new) ──│               │              │
       │                          │&lt;── 2. bind ───┴────────────────│               │              │
       │                          │─── write ────&gt;│                │               │              │
       │                          │&lt;── ack ───────│                │               │              │
       │                          │               │                │               │              │
       │                          │&lt;──────────────┼────────────────┼─ watch(bound)─│              │
       │                          │               │                │               │── 3. run ───&gt;│
       │                          │               │                │               │&lt;── status ───│
       │                          │&lt;── 4. update pod status ───────┴───────────────│              │
       │                          │─── write ────&gt;│                                │              │

```

#### Étapes détaillées du flux[13] :

1. **Création** : L'utilisateur soumet une demande de création de Pod à l'API Server via `kubectl`. L'API Server enregistre la ressource dans **etcd**[13].
2. **Ordonnancement** : Le **Scheduler**, qui surveille les nouveaux pods non assignés via un `watch`, détecte le Pod, choisit le nœud approprié, et renvoie une instruction d'assignation (`bind`) à l'API Server, qui met à jour l'état dans **etcd**[13].
3. **Exécution** : Le **Kubelet** du nœud concerné détecte que le Pod lui est désormais lié via son `watch`. Il sollicite le runtime local (**Docker/containerd**) pour lancer les conteneurs[13].
4. **Surveillance et feedback** : Le **Kubelet** surveille en continu l'état d'exécution et renvoie les mises à jour de statut à l'API Server, qui les répercute dans **etcd**[13].




# Chapitre 8 : Les Worker Nodes et le composant Kubelet

## **1. Présentation des Worker Nodes (Nœuds d'Exécution)**

### **Définition et rôle**

- **Définition** : Les **Worker Nodes** (ou **Kubernetes Nodes**) sont les machines physiques ou virtuelles composant le cluster et chargées d'exécuter effectivement les conteneurs et les charges de travail (_workloads_) utilisateurs[1].
- **Architecture globale** : Contrairement aux composants du _Control Plane_ qui orchestrent et prennent les décisions stratégiques pour le cluster[2], les Worker Nodes fournissent la puissance de calcul (_Compute_), la mémoire (_RAM_), le stockage (_Storage_) et l'interconnexion réseau (_Networking_) nécessaire aux Pods[3].

---

## **2. Le composant Kubelet**

### **Définition exacte et fonction**

- **Définition** : Le **Kubelet** est l'agent principal d'exécution fonctionnant sur chaque Worker Node du cluster[4].
- **Rôle central** : Il sert de pont de communication entre l'**API Server** du Control Plane et le moteur d'exécution de conteneurs (_Container Runtime_) local[4][5].

### **Fonctionnement détaillé et responsabilités du Kubelet**

1. **Écoute de l'API Server** : Le Kubelet surveille en permanence l'API Server (via un mécanisme de `watch`) pour détecter les Pods qui ont été attribués à son nœud par le Scheduler[4][6].
2. **Gestion du cycle de vie des conteneurs** :

- Lorsqu'un Pod lui est assigné, le Kubelet traduit la spécification déclarative du Pod (_PodSpec_) en instructions pour le runtime de conteneurs[4][6].
- Il ordonne au moteur d'exécution local (Docker, containerd, CRI-O, CoreOS rkt) d'effectuer le _pull_ des images réseau nécessaires et d'exécuter les conteneurs[5][6].

3. **Surveillance et rapport de statut (** **Feedback loop** **)** :

- Le Kubelet surveille l'état d'exécution et la santé des conteneurs du nœud[5][6].
- Il collecte les événements, les changements d'état et la consommation de ressources, puis renvoie ces informations à l'API Server afin de mettre à jour le statut global du Pod dans **etcd**[5][6].

---

## **3. Les composants complémentaires du Worker Node**

Pour assurer le fonctionnement complet des applications, chaque Worker Node embarque, aux côtés du Kubelet, plusieurs composants essentiels (Explication du schéma d'un Kubernetes Node)[1] :

### **Kube-Proxy**

- **Rôle** : Démon réseau fonctionnant sur chaque nœud du cluster[1].
- **Fonctionnement** :
    - Maintient et met à jour les règles réseau (_iptables_ / _IPVS_) sur la machine hôte.
    - Assure le routage du trafic et l'équilibrage de charge (_load balancing_) vers les Pods appropriés lorsqu'un **Service** Kubernetes est sollicité, que le trafic provienne de l'intérieur ou de l'extérieur du cluster[1].

### **cAdvisor (Container Advisor)**

- **Rôle** : Agent de collecte de métriques intégré directement dans le binaire du Kubelet[1].
- **Fonctionnement** :
    - Collecte, agrège et traite les données relatives à l'utilisation des ressources matérielles sur le nœud (consommation CPU, mémoire RAM, I/O disque, statistiques réseau par conteneur).
    - Met ces métriques à disposition du Kubelet et des outils de surveillance du cluster.

### **Moteur d'exécution des conteneurs (_Container Runtime_)**

- **Rôle** : Logiciel sous-jacent chargé de lancer et d'arrêter physiquement les conteneurs (ex. Docker, containerd, CRI-O)[5].
- **Interface CRI** : Le Kubelet interagit avec le Container Runtime via une interface standardisée appelée **CRI** (_Container Runtime Interface_).

### **Plugin Network (Interface CNI)**

- **Rôle** : Plugin réseau assurant l'interconnexion globale du cluster (ex. Flannel, Weave Net, Calico)[1].
- **Fonctionnement** :
    - Fournit le réseau d'imitation (_Overlay Network_) permettant à tous les Pods du cluster de communiquer entre eux de manière transparente d'un nœud à l'autre, avec une adresse IP unique attribuée à chaque Pod[1].

---

## **4. Schéma explicatif : Rôle du Kubelet dans le flux d'exécution d'un Pod**

Le flux d'interaction entre le Kubelet et les autres composants lors de la mise en route d'un Pod suit les étapes séquentielles ci-dessous[6] :

```
API Server              Scheduler               Kubelet             Container Runtime (Docker)
    │                       │                      │                            │
    │─── 1. watch(bound) ───┼─────────────────────&gt;│                            │
    │                       │                      │                            │
    │                       │                      │─── 2. docker run ─────────&gt;│
    │                       │                      │                            │
    │                       │                      │&lt;── 3. status (running) ────│
    │                       │                      │                            │
    │&lt;── 4. update status ──┴──────────────────────│                            │

```

### **Étapes détaillées :**

1. **Détection** : Le Kubelet détecte via l'API Server qu'un Pod « lié » (_bound_) vient de lui être attribué[6].
2. **Ordre d'exécution** : Le Kubelet transmet les instructions au Container Runtime local (ex: `docker run` ou appel gRPC CRI) pour préparer l'environnement du Pod (namespaces, cgroups, stockage) et lancer les conteneurs[5][6].
3. **Contrôle de santé** : Le Container Runtime exécute les conteneurs et renvoie l'état au Kubelet[6].
4. **Mise à jour du statut** : Le Kubelet renvoie le statut mis à jour (_Pending_ -> _Running_) à l'API Server, qui persiste cette information dans **etcd**[6].


# **Chapitres 9 & 10 : L'unité fonctionnelle Pod (Architecture, Réseau, Cycle de vie)**

## **Chapitre 9 : Concepts et architecture d'un Pod**

### **1. Définition et concept du Pod**

- **Définition exacte** : Un **Pod** est un groupe d'un ou plusieurs conteneurs, ayant du **stockage et du réseau partagés**, ainsi qu'une spécification sur la manière d'exécuter ces conteneurs[1].
- **Co-localisation et co-ordonnancement** : Les éléments d'un Pod sont toujours co-localisés (_co-located_) et co-ordonnancés (_co-scheduled_), et s'exécutent dans un **contexte partagé**[1].
- **Le contexte partagé** : Il s'agit d'un ensemble d'espaces de noms Linux (_Namespaces_), de groupes de contrôle (_Cgroups_) et de politiques d'isolement[1].

---

### **2. Gestion du réseau et communication au sein d'un Pod**

- **Adresse IP unique et ports** : Tous les conteneurs au sein d'un même Pod partagent une **adresse IP unique** et un **espace de ports commun**[1].
- **Communication interne au Pod** :
    - Les conteneurs d'un même Pod peuvent communiquer entre eux directement via `localhost`[1].
    - Ils peuvent également interagir en utilisant des mécanismes de communication inter-processus standard (**IPC**)[1].

---

### **3. Communication entre Pods distincts**

- **Isolation inter-Pods** : Les conteneurs appartenant à des Pods distincts possèdent des adresses IP différentes[4].
- **Absence d'IPC direct** : Ils ne peuvent pas communiquer par IPC sans une configuration spécifique explicite[4].
- **Routage** : Ils communiquent en général entre eux via les adresses IP respectives de leurs Pods[4].

---

### **4. Stockage partagé dans un Pod**

- **Volumes partagés** : Les applications installées dans les conteneurs d'un Pod ont accès à des **volumes partagés**[4].
- **Déclaration et montage** : Ces volumes sont définis au niveau de la spécification du Pod et sont mis à disposition pour être montés dans le système de fichiers de chaque conteneur/application du Pod[4].

---

## **Chapitre 10 : Cycle de vie et états du Pod**

### **1. Nature du Pod et identifiants (UID)**

- **Caractère éphémère** : Les Pods sont considérés comme des entités relativement **éphémères** (et non durables)[4].
- **Déroulement de la vie d'un Pod** :
    1. Les Pods sont créés par le cluster[2].
    2. Un identifiant unique universel (**UID**) leur est assigné[2].
    3. Ils sont ordonnancés sur un nœud (_Worker Node_) où ils restent jusqu'à leur arrêt (selon la politique de redémarrage / _restart policy_) ou leur suppression[2].
- **Gestion des pannes de nœuds** :
    - Si un nœud meurt, les Pods qui y étaient ordonnancés sont programmés pour être terminés[2].
    - **Non-réordonnancement d'un UID** : Un Pod donné (défini par son UID spécifique) n'est jamais « re-ordonnancé » sur un nouveau nœud[2].
    - **Remplacement** : En revanche, il peut être **remplacé** par un Pod identique, ayant le même nom si désiré, mais doté d'un **nouvel UID**[2].

---

### **2. Schéma explicatif : Diagramme des transitions d'états du Pod (Explication du schéma)**

Le cycle de vie d'un Pod est modélisé sous la forme d'un automate d'états représentant les étapes d'exécution et les voies de sortie possibles[2] :

```
                  ┌───────────┐
                  │  PENDING  │
                  └─────┬─────┘
                        │
            ┌───────────┼───────────┐
            │           │           │
            ▼           ▼           ▼
      ┌───────────┐ ┌───────┐ ┌───────────┐
      │  RUNNING  │ │FAILED │ │  UNKNOWN  │
      └─────┬─────┘ └───────┘ └─────┬─────┘
            │                       │
      ┌─────┴─────┐           ┌─────┴─────┐
      ▼           ▼           ▼           ▼
┌───────────┐ ┌───────┐ ┌───────────┐ ┌───────┐
│ SUCCEEDED │ │FAILED │ │ SUCCEEDED │ │FAILED │
└───────────┘ └───────┘ └───────────┘ └───────┘

```

#### **Analyse des voies de transition :**

- Un Pod débute obligatoirement à l'état **Pending**[2][3].
- Depuis **Pending**, il peut évoluer vers **Running**, ou basculer directement vers **Failed** (ex: échec d'image) ou **Unknown** (perte de contact réseau)[2].
- Depuis **Running**, une fois les tâches exécutées ou interrompues, le Pod termine soit en **Succeeded** (succès), soit en **Failed** (erreur), soit passe en **Unknown** si le nœud ne répond plus[2].

---

### **3. Les phases/états du cycle de vie d'un Pod (_Pod Phases_)**

Le statut du Pod fournit un résumé de sa phase actuelle dans le cycle de vie :

- **Pending** :
    - **Description** : Le Pod a été accepté par le système Kubernetes, mais une ou plusieurs images de conteneurs n'ont pas encore été créées ou téléchargées[3].
    - **Détail du temps** : Cela inclut le temps passé en attente d'être affecté à un nœud (_scheduling_) ainsi que le temps nécessaire pour télécharger les images du conteneur à travers le réseau[3].
- **Running** :
    - **Description** : Le Pod a été lié à un nœud et tous les conteneurs ont été créés[3].
    - **Conditions** : Au moins un conteneur est actuellement en cours d'exécution, ou est en phase de démarrage ou de redémarrage[3].
- **Succeeded** :
    - **Description** : Tous les conteneurs du Pod se sont terminés avec succès (code de sortie 0) et ne seront pas redémarrés[3].
- **Failed** :
    - **Description** : Tous les conteneurs du Pod se sont terminés, et au moins un conteneur s'est terminé en échec (code de sortie non nul ou arrêté par le système)[3].
- **Unknown** :
    - **Description** : Pour une raison quelconque, l'état du Pod n'a pas pu être obtenu[3].
    - **Cause fréquente** : Cela se produit généralement en raison d'un problème de communication réseau entre le Control Plane et le Kubelet du nœud hôte sur lequel le Pod est censé tourner[3].


# **Cours 2 : Gestion des Workloads, Stratégies de Déploiement et Services dans Kubernetes**

## **1. Concept de Workload et Cycle de Vie des Pods**

### **Définition d'un Workload**

- Un **workload** (charge de travail) est une application fonctionnant sur **Kubernetes**[1].
- Que le **workload** soit un composant unique ou un agrégat de composants, il fonctionne dans une série de **Pods**[1].
- La gestion d'une charge de travail représente un niveau d'abstraction plus élevé que la simple gestion de Pods individuels[2].
- Les principaux objets de gestion de **workload** dans Kubernetes sont[2] :
    - **Deployment**
    - **StatefulSet**
    - **DaemonSet**
    - **Job**
    - **CronJob**

### **Cycle de vie et nature des Pods**

- Les **Pods** sont considérés comme des entités éphémères et non durables[3].
- Le déroulement de la vie d'un Pod s'organise selon les étapes suivantes[3] :
    - Les Pods sont créés par le cluster[3].
    - Un identifiant unique universel (**UID**) leur est assigné[3].
    - Ils sont ordonnancés sur des **Worker Nodes** où ils restent jusqu'à leur arrêt (selon la politique de redémarrage / **restartPolicy**) ou leur suppression[3].
- **Gestion des pannes de nœuds** :
    - Si un nœud tombe en panne, les Pods ordonnancés sur ce nœud sont programmés pour être terminés[3].
    - Un Pod donné (défini par son **UID**) n'est jamais ré-ordonnancé sur un nouveau nœud[3].
    - Le Pod défaillant est remplacé par un Pod identique, possédant le même nom si désiré, mais avec un **nouvel UID**[3].

### **Automate des transitions d'états d'un Pod (Explication du schéma)**

Le cycle de vie d'un Pod évolue selon un diagramme d'états strict[4] :

- Le Pod débute obligatoirement dans l'état **Pending**[4].
- Depuis **Pending**, il bascule soit vers **Running**, soit directement vers **Failed** ou **Unknown**[4].
- Depuis **Running**, il termine son cycle soit en **Succeeded**, soit en **Failed**, soit passe en **Unknown** en cas de perte de connectivité[4].

```
                  ┌───────────┐
                  │  PENDING  │
                  └─────┬─────┘
                        │
            ┌───────────┼───────────┐
            │           │           │
            ▼           ▼           ▼
      ┌───────────┐ ┌───────┐ ┌───────────┐
      │  RUNNING  │ │FAILED │ │  UNKNOWN  │
      └─────┬─────┘ └───────┘ └─────┬─────┘
            │                       │
      ┌─────┴─────┐           ┌─────┴─────┐
      ▼           ▼           ▼           ▼
┌───────────┐ ┌───────┐ ┌───────────┐ ┌───────┐
│ SUCCEEDED │ │FAILED │ │ SUCCEEDED │ │FAILED │
└───────────┘ └───────┘ └───────────┘ └───────┘

```

### **Tableau détaillé des phases du Pod (Pod Phases)**

|Phase / Valeur|Description détaillée|
|---|---|
|**Pending**|Le Pod a été accepté par Kubernetes, mais une ou plusieurs images de conteneurs n'ont pas encore été créées ou téléchargées. Cela inclut le temps d'affectation au nœud et le téléchargement des images via le réseau[4].|
|**Running**|Le Pod a été affecté à un nœud et tous les conteneurs ont été créés. Au moins un conteneur est toujours en cours d'exécution, ou est en train de démarrer ou redémarrer[4].|
|**Succeeded**|Tous les conteneurs du Pod se sont terminés avec succès (code de sortie 0) et ne seront pas redémarrés[4].|
|**Failed**|Tous les conteneurs du Pod ont terminé, et au moins un conteneur a terminé en échec (statut non nul ou arrêté par le système)[1].|
|**Unknown**|Pour une raison quelconque, l'état du Pod ne peut pas être obtenu (en général en cas d'erreur de communication avec l'hôte du Pod)[1].|

---

## **2. La Ressource Deployment et la notion de ReplicaSet**

### **Le ReplicaSet**

- **Définition** : Un **ReplicaSet** (ensemble de réplicas) a pour but de maintenir un ensemble stable de Pods en cours d'exécution à un moment donné[5].
- **Objectif** : Garantir la disponibilité, l'auto-réparation (**self-healing**) et la scalabilité (**scalability**) d'un nombre identique de Pods[5].

### **Le Deployment**

- **Définition** : Le **Deployment** est un objet de niveau supérieur qui possède de façon déclarative un **ReplicaSet** ainsi que ses **Pods**[5].
- **Fonctionnement** :
    - On applique la configuration avec la commande[2] :

```
kubectl apply -f fichier.yaml

```

- Kubernetes sélectionne automatiquement un nœud pour exécuter le Pod (ou via l'option **nodeSelector**)[2].
- Kubernetes maintient les conteneurs en fonctionnement en cas de panne de nœud[2].

### **Hiérarchie des objets (Explication du schéma)**

```
Deployment (Gestion des Rolling Updates et Rollbacks)
   └── ReplicaSet (Garantie de disponibilité et Scalabilité)
         ├── Pod 1 (Conteneur)
         ├── Pod 2 (Conteneur)
         └── Pod N (Conteneur)

```

### **Cas d'utilisation typiques d'un Deployment**

- **Déploiement initial** : Créer un **Deployment** pour instancier un **ReplicaSet**, qui crée ensuite les Pods en arrière-plan[6].
- **Mise à jour d'application** : Déclarer le nouvel état des Pods en mettant à jour le **PodTemplateSpec**. Un nouveau **ReplicaSet** est créé et le **Deployment** gère la migration progressive des Pods de l'ancien vers le nouveau **ReplicaSet**. Chaque nouveau **ReplicaSet** incrémente la révision du déploiement[6].
- **Rollback (Retour arrière)** : Revenir à une révision de déploiement antérieure si l'état actuel est instable[6].
- **Mise à l'échelle (Scaling)** : Augmenter ou réduire le nombre de réplicas pour traiter la charge[6].
- **Détection de blocage** : Utiliser le statut du **Deployment** comme indicateur d'un processus bloqué[6].
- **Nettoyage** : Purger les anciens **ReplicaSets** inutiles[6].

### **Syntaxe YAML d'un Deployment (Exemple Nginx)**

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.14.2
          ports:
            - containerPort: 80

```

*(Code YAML issu du slide 5)*[5].

---

## **3. Les Stratégies de Déploiement Applicatif**

Il existe deux stratégies de mise à jour (**rollout**) natives dans Kubernetes[7], complétées par des stratégies avancées nécessitant des composants complémentaires[8][9].

### **1. Stratégie Recreate (NATIVE)**

- **Principe** : Arrêter simultanément tous les anciens Pods avant de créer les nouveaux Pods[7].
- **Impact** : Entraîne une interruption de service (**downtime**) pendant la phase de transition[7].

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: recreate-strategy
spec:
  strategy:
    type: Recreate
  selector:
    matchLabels:
      app: web-app-recreate-strategy
      version: nanoserver-1809
  replicas: 2
  template:
    metadata:
      labels:
        app: web-app-recreate-strategy
        version: nanoserver-1809
    spec:
      containers:
        - name: web-app-recreate-strategy
          image: hello-world:nanoserver-1809

```

*(Code YAML issu du slide 7)*[7].

### **2. Stratégie RollingUpdate (NATIVE - PAR DÉFAUT)**

- **Principe** : Mise à jour continuous avec arrêt progressif des anciens Pods les uns après les autres et création simultanée des nouveaux Pods[10].
- **Options d'affinage** :
    - **maxSurge** : Nombre maximal de Pods pouvant être créés au-dessus du nombre de réplicas souhaité pendant la mise à jour (valeur absolue ou pourcentage, valeur par défaut : **25%**)[11].
    - **maxUnavailable** : Nombre maximal de Pods pouvant être indisponibles pendant le processus de mise à jour (valeur absolue ou pourcentage, valeur par défaut : **25%**)[8].

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: rollingupdate-strategy
spec:
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 2
      maxUnavailable: 1
  selector:
    matchLabels:
      app: web-app-rollingupdate-strategy
      version: nanoserver-1709
  replicas: 2
  template:
    metadata:
      labels:
        app: web-app-rollingupdate-strategy
        version: nanoserver-1709
    spec:
      containers:
        - name: web-app-rollingupdate-strategy
          image: hello-world:nanoserver-1709

```

*(Code YAML issu des slides 8-9)*[10][11].

### **3. Stratégie Blue-Green (AVANCÉE)**

- **Principe** :
    - **Blue** représente la version actuelle en production[9].
    - **Green** représente la nouvelle version déployée en parallèle[9].
    - Le trafic reste acheminé vers **Blue** pendant que **Green** est testé. Une fois les tests validés, le trafic est basculé intégralement vers **Green**[12].
- **Avantages et Inconvénients** :
    - _Avantage_ : Une seule version reçoit le trafic en direct à la fois, bascule instantanée[12].
    - _Inconvénient_ : Très coûteux car nécessite le **double de ressources** (2 Deployments, 2 Services)[12].
    - _Complexité_ : Gestion difficile pour les applications avec état (**stateful**)[12].

### **4. Stratégie Canary (AVANCÉE)**

- **Principe** : Déploiement progressif où la nouvelle version est déployée à côté de l'ancienne mais ne reçoit qu'un petit pourcentage du trafic réel de production[13].
- **Éléments de planification d'un déploiement Canary** :
    - **Étapes** : Définir le pourcentage initial d'utilisateurs et le nombre d'étapes de progression[14].
    - **Durée** : Définir le temps d'observation (peut s'étendre sur plusieurs jours ou semaines)[14].
    - **Métriques** : Sélectionner les indicateurs de suivi (performances, taux d'erreur, codes HTTP 200 vs 5xx)[14].
    - **Évaluation** : Définir les critères de validation ou d'échec de la version[14].

### **5. Stratégie A/B Testing (AVANCÉE)**

- **Principe** : Comparaison expérimentale contrôlée entre deux versions[15] :
    - **Groupe A** (groupe témoin) : reçoit la version standard[15].
    - **Groupe B** (groupe de traitement) : reçoit la nouvelle version avec des modifications ciblées[15].
- **Acheminement** : Basé sur des critères applicatifs précis (en-têtes HTTP, cookies, géolocalisation)[9].
- **Prérequis** : Non supporté de manière native par Kubernetes seul ; nécessite un reverse proxy ou service mesh avancé (**Istio**, **Traefik**, **NGINX**, **HAProxy**, **Seldon-core**, **Prometheus**, **Grafana**)[9][15].

---

## **4. Exposition Réseau et la Ressource Service**

### **Problématique réseau liée aux Deployments**

- Les Pods étant éphémères, leurs adresses IP changent continuellement[16].
- Si un ensemble de Pods backend fournit des services à un ensemble de Pods frontend, le frontend ne peut pas s'appuyer sur des adresses IP fixes[16][17].

### **Définition du Service**

- **Définition** : Un **Service** est une abstraction Kubernetes qui définit un ensemble logique de Pods et une politique d'accès réseau[17].
- **Sélection** : L'association entre le Service et les Pods est réalisée dynamiquement via un **selector** (basé sur les **labels**)[17].

### **Syntaxe YAML d'un Service**

```
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    matchLabels:
      run: A
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376

```

*(Code YAML issu du slide 18)*[18].

- _Explication_ : Ce service nommé `my-service` écoute sur le port **80** et redirige le trafic TCP vers le port **9376** des Pods portant le label `run: A`[18].

---

## **5. Les 4 Types de Services Kubernetes**

### **1. ClusterIP (Par Défaut)**

- **Définition** : Expose le service sur une adresse IP interne au cluster[19][20].
- **Accessibilité** : Uniquement accessible depuis l'intérieur du cluster[19][20].
- **Accès externe de débogage** : Possible temporairement via le proxy Kubernetes (**kubectl proxy**)[20].

### **2. NodePort**

- **Définition** : Expose le service sur l'adresse IP de chaque **Worker Node** en ouvrant un port statique[19][21].
- **Plage de ports réservée** : **30000 à 32767**[21].
- **Mécanisme** : Crée automatiquement un service **ClusterIP** sous-jacent[19]. Accessible depuis l'extérieur via `IP_du_Noeud:NodePort`[19].

```
apiVersion: v1
kind: Service
metadata:
  name: my-nodeport-service
spec:
  type: NodePort
  selector:
    app: my-app
  ports:
    - name: http
      port: 7500
      targetPort: 8800
      nodePort: 30005
      protocol: TCP

```

*(Code YAML issu du slide 22)*[22].

### **3. LoadBalancer**

- **Définition** : Expose le service vers l'extérieur en instanciant un équilibreur de charge géré par le fournisseur Cloud (**Cloud Provider**)[19][22].
- **Types d'équilibreurs de charge** :
    - **Load Balancer L4 (Network Load Balancer)** : Redirige le trafic basé uniquement sur la couche réseau/transport (TCP/UDP)[23].
    - **Load Balancer L7 (Application Load Balancer)** : Redirige le trafic en fonction des paramètres applicatifs (HTTP, HTTPS, SSL, en-têtes, cookies, URLs)[23].
- **Limitations du Service LoadBalancer** :
    - Gère uniquement des adresses IP (pas directement des noms de domaines)[24].
    - Nécessite un équilibreur de charge dédié par service (coûteux en ressources et en configuration)[24].

### **4. ExternalName**

- **Définition** : Mappe le service vers un nom DNS externe en renvoyant un enregistrement **CNAME**[19][25].
- **Mécanisme** : Aucun proxy réseau n'est mis en place[19]. Permet aux Pods du cluster d'accéder à un service externe via un nom alias (ex: `foo.bar.example.com`)[19][25].

### **Tableau récapitulatif des Types de Services**

|Type de Service|Portée de l'accès|IP attribuée|Cas d'usage principal|
|---|---|---|---|
|**ClusterIP**|Interne au cluster|IP virtuelle interne|Communication microservices interne (ex: BDD)[19][20].|
|**NodePort**|Externe / Interne|IP du nœud + Port 30000-32767|Tests, débogage ou exposition directe simple[19][21].|
|**LoadBalancer**|Externe (Internet)|IP publique externe Cloud|Exposition de production sur Cloud Provider[19][22].|
|**ExternalName**|Externe (DNS)|Enregistrement CNAME|Redirection vers un composant hors du cluster[19][25].|

---

## **6. L'Ingress et l'Ingress Controller**

### **Définition de l'Ingress**

- **Définition** : L'**Ingress** (traduit par « entrée ») est un objet Kubernetes qui gère l'accès externe (généralement HTTP et HTTPS) aux services situés dans le cluster[24][26].

### **Fonctionnalités fournies par l'Ingress**

- **Équilibrage de charge** (Load balancing)[26].
- **Terminaison SSL/TLS**[26].
- **Hébergement virtuel basé sur le nom** (Name-based virtual hosting)[26].
- **Routage par chemin d'URL** (Path-based routing)[26].

### **Architecture Ingress (Explication du schéma)**

```
Client (Internet)
   │
   ▼ (HTTP / HTTPS)
Ingress Controller (ex: NGINX, Traefik)
   ├── test.labs.iut-larochelle.fr/foo ──&gt; Service 1 (Port 4200) ──&gt; Pods
   └── test.labs.iut-larochelle.fr/bar ──&gt; Service 2 (Port 8080) ──&gt; Pods

```

### **Exemple 1 : Ingress avec Hébergement Virtuel Simple**

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: name-virtual-host-ingress
spec:
  rules:
    - host: test.labs.iut-larochelle.fr
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: testlabs
                port:
                  number: 80

```

*(Code YAML issu du slide 27)*[27].

### **Exemple 2 : Ingress avec Répartition par Chemin (Simple Fanout)**

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: simple-fanout-example
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
    - host: test.labs.iut-larochelle.fr
      http:
        paths:
          - path: /foo
            pathType: Prefix
            backend:
              service:
                name: service1
                port:
                  number: 4200
          - path: /bar
            pathType: Prefix
            backend:
              service:
                name: service2
                port:
                  number: 8080

```

*(Code YAML issu du slide 28)*[28].