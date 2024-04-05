# TP Docker

Ce projet contient une application Flask simple qui affiche une page de bienvenue accompagné d'un numéro de version. Le but de ce projet est de découvrir l'utilisation de pipeline CI/CD avec l'outils GitHub Actions.

Pour cela nous allons effectuer les étapes suivantes : 

* Mise en place de l'application Flask dans un fichier app.py
* Mise en place de test unitaire dans un fichier test_app.py
* Création d'une Web App sur Azure
* Mise en place d'une pipeline CI/CD avec l'outils GitHub Actions.
    * Cela en trois parties :
        * Build de l'application
        * Test de l'application
        * Déploiement en production sur Azure Web App

-----------------------------------

## Prérequis

- Git 
- Python

-----------------------------------

#### 1. Cloner le repo Git
Ici l'application sera accessible sur l'ip : 127.0.0.1 sur le port 8000.
http://127.0.0.1:8000

------------------------------------

## Construire un container Docker avec une application personnalisée en utilisant le fichier Dockerfile.

#### 1. Créer un app.py

Exemple de fichier :

```
from flask import Flask, request
app = Flask(__name__)
@app.route("/")
def home():
    return "Welcome to the Flask CI/CD Demo! This is V7!"
@app.route("/new-deployment")
def new_deployment():
    return "New deployment from CI/CD"

@app.route("/info")
def info():
    user_agent = request.headers.get('User-Agent')
    return f"Your user agent is: {user_agent}"
if __name__ == "__main__":
    app.run(debug=True, host='0.0.0.0', port='5000')
```
#### 1. Créer un test_app.py

Exemple de fichier :

```
import unittest
from app import app

class FlaskTestCase(unittest.TestCase):
    def setUp(self):
        self.app = app.test_client()
        self.app.testing = True
        
    def test_home_status_code(self):
        response = self.app.get('/')
        self.assertEqual(response.status_code, 200)
        
    def test_info_status_code(self):
        response = self.app.get('/info')
        self.assertEqual(response.status_code, 200)
    
if __name__ == '__main__':
    unittest.main()
```

#### 3. Créer la WebApp sur Azure

Lien vers la documentation Azure consacré à la création d'une application web : [ici](https://learn.microsoft.com/fr-fr/azure/static-web-apps/get-started-portal?tabs=vanilla-javascript&pivots=github)

Se rendre dans le centre de déploiement et connecter le compte GitHub lié au projet

## Mise en place d'une pipeline CI/CD avec l'outils GitHub Actions.

#### 1. Création du Workflow :

Récuperer le workflow mis à disposition de Azure précédement.
Modifier ce fichier pour ajuster les étapes : 
* Build
* Test
* Deploy

#### 2. Tester la pipeline CI/CD

Comme configuré dans le workflow, la pipeline se déclenche automatiquement lors d'un push. On peut vérifier si tout fonctionne correctement en modifiant le app.py, puis en faisant un git push.

Se rendre dans la partie Actions de son repositorie GitHub et s'assurer que les différentes étapes se passent correctement.

Une fois terminé se connecté sur le lien indiqué à la dernière étape de notre pipeline (Deploy) et s'assurer que l'application est fonctionnel.
## Auteurs

- Boulen Pierre
- Lebreton Tom
