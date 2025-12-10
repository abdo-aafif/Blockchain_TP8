# 🚀 TP 8: Hello World DApp (Flutter & Ethereum/Truffle)

Ce projet est un guide pratique pour le développement d'une application décentralisée (DApp) simple, connectant une interface utilisateur mobile développée en **Flutter** à un **Smart Contract Ethereum** déployé localement via **Truffle** et **Ganache**.

## 🎯 Objectifs du TP

*   Mettre en place l'environnement de développement Blockchain (Node.js, Truffle, Ganache).
*   Créer et initialiser un projet combinant Flutter et Truffle.
*   Rédiger et compiler un contrat intelligent simple en Solidity (`HelloWorld.sol`).
*   Déployer le contrat sur la blockchain personnelle Ganache via un script de migration.
*   Écrire et exécuter des tests unitaires pour le contrat avec Truffle.
*   Lier le contrat intelligent à l'application Flutter en utilisant la librairie `web3dart`.
*   Créer une interface utilisateur Flutter pour interagir avec l'état du contrat (lecture et écriture).

## 🛠️ Prérequis

Assurez-vous d'avoir les outils suivants installés:

1.  **Node.js**
2.  **Flutter** (pour le développement mobile).
3.  **Truffle**
4.  **Ganache** (Blockchain personnelle Ethereum)

### 1. Installation de Truffle

Une fois Node.js installé, vous pouvez installer Truffle globalement:

```bash
npm install -g truffle
```

### 2. Installation de Ganache

Téléchargez Ganache, la blockchain personnelle utilisée pour le développement et les tests Ethereum.

## ⚙️ Mise en place du Projet

### 1. Création du Projet

1.  Créez un projet Flutter de base :
    ```bash
    flutter create hello_world
    cd hello_world
    ```
2.  Initialisez Truffle dans le répertoire du projet Flutter:
    ```bash
    truffle init
    ```

### 2. Structure du Contrat (`contracts/HelloWorld.sol`)

Le contrat intelligent sert de logique *back-end* et stocke une simple variable `yourName`.

Créez le fichier `contracts/HelloWorld.sol` :

```solidity
pragma solidity ^0.5.9;

contract HelloWorld {
    string public yourName;

    // Constructeur : Initialise yourName à "Unknown" lors du déploiement
    constructor() public {
        yourName = "Unknown";
    }

    // Fonction : Définit une nouvelle valeur pour yourName
    function setName(string memory nm) public {
        yourName = nm;
    }
}
```

### 3. Compilation et Migration

1.  **Compilation** :
    ```bash
    truffle compile
    ```

2.  **Script de Migration** (`migrations/2_deploy_contracts.js`):
    ```javascript
    const HelloWorld = artifacts.require("HelloWorld");

    module.exports = function (deployer) {
      deployer.deploy(HelloWorld);
    };
    ```

3.  **Configuration Ganache** :
    Modifiez `truffle-config.js` pour pointer vers le bon hôte/port de Ganache (ex: 7545).

4.  **Migration** :
    ```bash
    truffle migrate
    ```

### 4. Tests

Créez le fichier `test/helloWorld.js` :

```javascript
const HelloWorld = artifacts.require("HelloWorld");

contract("HelloWorld", () => {
  it("Hello World Testing", async () => {
    const helloWorld = await HelloWorld.deployed();
    await helloWorld.setName("User Name");
    const result = await helloWorld.yourName();
    assert(result === "User Name");
  });
});
```

Exécutez les tests:

```bash
truffle test
```

## 📱 Application Flutter (DApp)

La DApp Flutter utilise la librairie `web3dart`.

### 1. Dépendances (`pubspec.yaml`)

```yaml
dependencies:
  flutter:
    sdk: flutter
  provider: ^4.3.3
  web3dart: ^1.2.3
  http: ^0.12.2
  web_socket_channel: ^1.2.0

flutter:
  assets:
    - src/artifacts/HelloWorld.json
```

### 2. Logique et UI

*   **ContractLinking** : Gère la connexion à Ganache et les appels au contrat.
*   **HelloUI** : Interface pour afficher et modifier le nom.

## ▶️ Exécution

1.  Lancez Ganache.
2.  Migrez le contrat.
3.  Lancez l'app Flutter :
    ```bash
    flutter run
    ```
