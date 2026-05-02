# 🎮 GameStore — Lab Web Service PHP + Android

Application de gestion de jeux vidéo développée dans le cadre d'un lab académique.  
Elle repose sur une architecture **3-tiers** : base de données MySQL, web service PHP, et application Android.

---

## 🗂️ Architecture du projet

```
gamestore/                  ← Projet PHP (XAMPP)
├── classes/
│   └── Jeu.php             ← Modèle de données
├── connexion/
│   └── DbManager.php       ← Connexion PDO à MySQL
├── dao/
│   └── IDao.php            ← Interface CRUD
├── service/
│   └── JeuService.php      ← Logique métier
└── ws/
    ├── ajouterJeu.php      ← Web Service POST
    └── chargerJeux.php     ← Web Service GET

GameStore/                  ← Projet Android Studio
├── Jeu.java                ← Bean / modèle
├── AddJeu.java             ← Activité d'ajout
├── MainActivity.java       ← Activité principale
└── res/
    ├── layout/
    └── xml/
        └── network_security_config.xml
```

---

## 🛠️ Technologies utilisées

| Couche | Technologie |
|---|---|
| Base de données | MySQL (XAMPP) |
| Backend | PHP 8 avec PDO |
| Format d'échange | JSON |
| Application mobile | Android (Java) |
| Requêtes HTTP | Volley |
| Parsing JSON | Gson |

---

## 🗄️ Partie 1 — Base de données MySQL

**Base de données :** `gamestore_db`  
**Table :** `Jeu`

```sql
CREATE DATABASE gamestore_db;

USE gamestore_db;

CREATE TABLE Jeu (
  id          INT AUTO_INCREMENT PRIMARY KEY,
  titre       VARCHAR(100),
  plateforme  VARCHAR(50),
  genre       VARCHAR(50),
  editeur     VARCHAR(50)
);

INSERT INTO Jeu (titre, plateforme, genre, editeur)
VALUES 
  ('God of War',    'PS5',  'Action', 'Sony'),
  ('Halo Infinite', 'Xbox', 'FPS',    'Microsoft');
```

---

## 🐘 Partie 2 — Web Service PHP

### Prérequis
- XAMPP installé et démarré (Apache + MySQL)
- Dossier projet placé dans `C:\xampp\htdocs\gamestore\`

### Fichiers clés

**`connexion/DbManager.php`** — Connexion PDO
```php
$this->pdo = new PDO(
    "mysql:host=localhost;dbname=gamestore_db;charset=utf8",
    "root", ""
);
```

**`ws/chargerJeux.php`** — Retourne tous les jeux en JSON (GET)  
**`ws/ajouterJeu.php`** — Ajoute un jeu et retourne la liste mise à jour (POST)

### Test des endpoints

**GET — Lister les jeux**
```
GET http://localhost/gamestore/ws/chargerJeux.php
```

Réponse attendue :
```json
[
  {"id":"1","titre":"God of War","plateforme":"PS5","genre":"Action","editeur":"Sony"},
  {"id":"2","titre":"Halo Infinite","plateforme":"Xbox","genre":"FPS","editeur":"Microsoft"}
]
```

**POST — Ajouter un jeu**
```
POST http://localhost/gamestore/ws/ajouterJeu.php
Content-Type: application/x-www-form-urlencoded

titre=Elden Ring&plateforme=PC&genre=RPG&editeur=Bandai Namco
```

---

## 📱 Partie 3 — Application Android

### Prérequis
- Android Studio installé
- API minimum : 26
- Langage : Java

### Dépendances (`build.gradle`)
```gradle
implementation 'com.android.volley:volley:1.2.1'
implementation 'com.google.code.gson:gson:2.10.1'
```

### Permission Internet (`AndroidManifest.xml`)
```xml
<uses-permission android:name="android.permission.INTERNET" />
```

### Configuration réseau (`res/xml/network_security_config.xml`)
Nécessaire pour Android 9+ afin d'autoriser le trafic HTTP local :
```xml
<network-security-config>
    <domain-config cleartextTrafficPermitted="true">
        <domain includeSubdomains="true">10.0.2.2</domain>
    </domain-config>
</network-security-config>
```

> ⚠️ `10.0.2.2` est l'adresse qui pointe vers `localhost` depuis l'émulateur Android.

### Fonctionnement
1. L'utilisateur remplit le formulaire (titre, éditeur, plateforme, genre)
2. Au clic sur **Ajouter**, une requête POST est envoyée via **Volley**
3. La réponse JSON est parsée avec **Gson** en liste d'objets `Jeu`
4. Les résultats sont affichés dans le **Logcat** (ou une ListView)

---

## 🚀 Lancer le projet

1. Démarrer **XAMPP** → activer Apache et MySQL
2. Copier le dossier `gamestore/` dans `C:\xampp\htdocs\`
3. Importer la base de données via **phpMyAdmin**
4. Lancer l'application Android sur l'émulateur
5. Tester l'ajout d'un jeu depuis l'interface

---

## 👤 Auteur

Yasser Chettour Étudiant en Ingénierie Cybersécurité à l'École Nationale des Sciences Appliquées de Marrakech,Université Cadi Ayyad.
