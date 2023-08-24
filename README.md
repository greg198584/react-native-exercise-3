# React native - Exercises - 3

## Exercice 6: Intégration d'une API

Dans cette partie, nous allons créer une simple API avec Node.js et Express pour simuler l'obtention de données de profil d'un utilisateur. Nous intégrerons ensuite cette API dans notre application React Native.

### Étape 1 : Initialisation du serveur Node.js

Pour commencer, nous allons initialiser un nouveau projet Node.js. Assurez-vous d'avoir Node.js installé sur votre machine. Ouvrez un terminal et tapez les commandes suivantes :

```
mkdir profile-api
cd profile-api
npm init -y
```

Cela crée un nouveau dossier pour notre projet d'API, navigue dans ce dossier, et initialise un nouveau projet Node.js.

### Étape 2 : Installation d'Express

Ensuite, nous allons installer Express, un cadre rapide, sans opinion, minimaliste pour Node.js qui nous permettra de construire notre API. Tapez la commande suivante dans votre terminal :

```
npm install express
```

### Étape 3 : Création de notre API

Maintenant, nous allons créer un fichier pour notre serveur Express et notre API. Créez un nouveau fichier nommé `server.js` et ajoutez le code suivant :

```javascript
const express = require('express');
const app = express();
const port = 3000;

// Simulated user data
const userData = {
    name: 'John Doe',
    age: 32,
    occupation: 'Developer',
};

app.get('/profile', (req, res) => {
    res.json(userData);
});

app.listen(port, () => {
    console.log(`Server listening at http://localhost:${port}`);
});
```

Ce script crée un nouveau serveur Express et une route API `/profile` qui renvoie un objet JSON contenant des données d'utilisateur simulées.

### Étape 4 : Lancement du serveur

Pour lancer le serveur, tapez la commande suivante dans votre terminal :

```
node server.js
```

Votre serveur est maintenant en marche et écoute sur le port 3000. Si vous naviguez vers `http://localhost:3000/profile` dans votre navigateur, vous devriez voir les données d'utilisateur simulées.

### Étape 5 : Intégration de l'API dans l'application React Native

Nous allons maintenant modifier notre `ProfileScreen` dans notre application React Native pour obtenir et afficher les données de l'API lorsque l'écran est affiché. Nous utiliserons la fonction `fetch` intégrée pour cela.

Ajoutez le code suivant dans `ProfileScreen.js` :

```javascript
import React, { useEffect, useState } from 'react';

// ... reste import ...

const ProfileScreen = () => {
    const [userData, setUserData] = useState(null);

    useEffect(() => {
        fetch('http://localhost:3000/profile')
            .then(response => response.json())
            .then(data => setUserData(data));
    }, []);

    if (!userData) {
        return <Text>Loading...</Text>;
    }

    return (
        <View style={styles.container}>
           // ... reste du code ...
            <Text style={styles.text}>Name: {userData.name}</Text>
            <Text style={styles.text}>Age: {userData.age}</Text>
            <Text style={styles.text}>Occupation: {userData.occupation}</Text>
        </View>
    );
};


export default ProfileScreen;
```

Maintenant, lorsque vous naviguez vers le `ProfileScreen`, il devrait automatiquement obtenir et afficher les données d'utilisateur de notre API.


**NB** : Dans une situation réelle, vous auriez à gérer les erreurs de réseau et à sécuriser votre API. Ce guide est simplifié pour une meilleure compréhension des concepts de base.re de visites est partagé entre ces deux écrans grâce au contexte.