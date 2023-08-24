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
let userData = {
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



### Étape 6 : Ajout d'une route POST pour enregistrer les vues

Nous allons maintenant ajouter une nouvelle route POST à notre serveur Node.js pour gérer l'ajout des vues de profil.

Dans votre fichier `server.js`, ajoutez le code suivant :

```javascript
app.use(express.json());

app.post('/profile/views', (req, res) => {
    const { count } = req.body;
    // Ajout du nombre de vues (simulé ici, mais dans une application réelle, vous enregistreriez cela dans une base de données)
    userData.views = (userData.views || 0) + count;
    res.json({ success: true });
});
```

Ceci crée une nouvelle route POST qui accepte un paramètre `count` dans le corps de la requête et ajoute ce nombre aux vues de profil.

### Étape 7 : Ajout d'une fonction pour effectuer une requête POST dans React Native

Dans votre composant `ProfileScreen` de l'application React Native, nous allons ajouter une fonction qui effectue une requête POST à notre API pour enregistrer le nombre de vues.

Ajoutez le code suivant dans `ProfileScreen.js` :

```javascript
const addViewCount = () => {
    fetch('http://localhost:3000/profile/views', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify({ count }),
    });
};

// ...

<Button title="Ajouter vue" onPress={addViewCount} />
```

Cette fonction envoie une requête POST à notre API avec le nombre de visites actuel.

### Étape 8 : Testez votre application

Vous pouvez maintenant lancer votre serveur Node.js et tester l'application React Native. Naviguez vers le `ProfileScreen`, et vous devriez voir les données d'utilisateur et le bouton "Ajouter vue". En appuyant sur ce bouton, vous envoyez une requête POST à votre API, et le nombre de vues est mis à jour côté serveur.

**NB** : Dans une application réelle, vous voudrez peut-être gérer les erreurs de réseau, afficher un message de succès ou d'échec, et mettre à jour l'interface utilisateur en conséquence. Ce guide est simplifié pour vous aider à comprendre les concepts de base de l'intégration d'une API dans React Native et Node.js.

Ces étapes concluent l'exercice d'intégration d'une API simple dans une application React Native. 

Vous avez appris à créer une API avec Node.js et Express, et comment interagir avec cette API depuis une application React Native.