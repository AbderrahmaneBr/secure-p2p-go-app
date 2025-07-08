# GoChat

Une application de chat en TUI (Interface Utilisateur en Terminal) chiffrée de bout en bout, développée en Go. GoChat est une implémentation client du protocole CloseCircle, conçue pour une communication sécurisée sur un réseau local.

## Avertissement

Ce projet a été créé dans le cadre d'un projet de cours pour CISC 468 (Introduction à la Cryptographie). Bien que l'accent ait été mis sur la sécurité, il n'est pas conçu pour un usage public général. Il fonctionne principalement sur un réseau local (LAN) et ne prend pas en charge les conversations simultanées.

## Concepts Clés

Avant de commencer, voici une explication de certains termes techniques utilisés dans ce projet :

- **Chiffrement de Bout en Bout (End-to-End Encryption - E2EE) :** C'est une méthode de communication sécurisée qui empêche des tiers d'accéder aux données transférées d'un appareil à un autre. Dans GoChat, lorsque vous envoyez un message, il est chiffré sur votre appareil et n'est déchiffré que sur l'appareil de votre contact. Personne entre les deux, pas même un serveur central (qui n'existe pas ici), ne peut lire le message.

- **Peer-to-Peer (P2P / Pair-à-Pair) :** Dans un réseau P2P, les appareils (ou "pairs") communiquent directement entre eux sans avoir besoin d'un serveur central. Pour GoChat, cela signifie qu'Alice et Bob se connectent directement l'un à l'autre sur le réseau local. Cela améliore la confidentialité mais limite la communication aux appareils se trouvant sur le même réseau.

- **TUI (Terminal User Interface / Interface Utilisateur en Terminal) :** C'est une interface qui s'exécute entièrement dans votre terminal ou votre console. Au lieu d'utiliser la souris avec des boutons et des fenêtres graphiques, vous interagissez avec l'application en utilisant votre clavier (touches fléchées, Entrée, etc.), le tout dans un environnement textuel.

## Fonctionnalités

- **Chiffrement de Bout en Bout :** Les messages sont chiffrés pour garantir la confidentialité.
- **Interface Utilisateur en Terminal :** Une interface propre et basée sur le terminal pour discuter.
- **Peer-to-Peer :** Fonctionne directement entre les pairs sur un réseau local.
- **Profils Sécurisés :** Les profils utilisateurs sont chiffrés localement.

## Démarrage (Méthode Recommandée)

La manière la plus simple d'exécuter GoChat est d'utiliser Docker et Docker Compose, qui configureront deux clients (`alice` et `bob`) dans un réseau simulé.

### Prérequis

- [Docker](https://www.docker.com/products/docker-desktop/) installé et en cours d'exécution.
- [Docker Compose](https://docs.docker.com/compose/install/) (généralement inclus avec Docker Desktop).

### Étape 1 : Construire et Lancer les Conteneurs

Depuis le répertoire racine du projet, exécutez la commande suivante. Cela construira l'image Docker et lancera deux conteneurs, `gochat-alice-1` et `gochat-bob-1`, en arrière-plan.

```sh
docker-compose up -d
```

### Étape 2 : Accéder aux Terminaux des Clients

Vous devez ouvrir **deux fenêtres de terminal distinctes** pour interagir avec Alice et Bob.

- **Dans votre premier terminal**, connectez-vous au conteneur d'Alice :

  ```sh
  docker exec -it gochat-alice-1 /bin/sh
  ```

- **Dans votre second terminal**, connectez-vous au conteneur de Bob :
  ```sh
  docker exec -it gochat-bob-1 /bin/sh
  ```

Vous aurez alors une invite de commande (`/ #`) à l'intérieur de chaque conteneur.

### Étape 3 : Démarrer l'Application de Chat

Dans **les deux** terminaux (celui d'Alice et celui de Bob), démarrez le client GoChat en exécutant :

```sh
/app/main
```

L'interface TUI interactive se lancera.

### Étape 4 : Comment Discuter

L'application est pilotée par un menu. Utilisez vos touches fléchées et la touche Entrée pour naviguer.

1.  **Créer les Profils :** La première fois que vous lancez l'application, il vous sera demandé de créer un profil.

    - Dans le terminal d'Alice, créez un profil pour `alice`.
    - Dans le terminal de Bob, créez un profil pour `bob`.

2.  **S'enregistrer Mutuellement :** Pour discuter, vous devez vous enregistrer mutuellement comme contacts en utilisant un mot de passe secret partagé.

    - Dans le terminal d'Alice, sélectionnez "Register A Contact", entrez `bob` comme nom, et fournissez un mot de passe partagé (ex: `motdepasse123`).
    - Dans le terminal de Bob, sélectionnez "Register A Contact", entrez `alice` comme nom, et fournissez le **même mot de passe partagé**.

3.  **Se Connecter et Discuter :**
    - Dans l'un des terminaux (par exemple, celui d'Alice), sélectionnez "Connect to a Contact" et choisissez l'autre utilisateur dans la liste.
    - Une fois la connexion établie, les deux utilisateurs peuvent sélectionner "Enter Chat" dans le menu principal pour commencer à échanger des messages.

## Développement Local (Méthode Alternative)

Vous pouvez également exécuter le client directement sur votre machine locale si Go est installé.

### Prérequis

- [Go (version 1.22 ou plus récente)](https://go.dev/doc/install) installé.

### Exécuter le Client

Ouvrez un terminal et exécutez la commande suivante depuis la racine du projet :

```sh
go run ./cmd/go-client/main.go
```

Cela lancera un client. Pour discuter, vous devrez lancer une seconde instance dans un terminal séparé. L'application créera un fichier `profile.json` dans le répertoire où vous exécutez la commande.
