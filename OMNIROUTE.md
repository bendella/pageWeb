# OmniRoute — mémo d'installation

OmniRoute est une passerelle ("gateway") pour les IA : au lieu d'avoir un compte
et une clé différente pour Claude, ChatGPT, Gemini, etc., tu installes OmniRoute
sur ta machine et tu lui parles à **une seule adresse**. C'est lui qui redirige
vers le bon fournisseur, et qui bascule automatiquement sur un autre si l'un
tombe en panne.

- Projet : https://github.com/diegosouzapw/OmniRoute
- Paquet npm : `omniroute`
- Version installée lors de cette session : **3.8.50**

## 1. Ce qu'il faut avant

Node.js en version **22.22.2 ou plus** (mais pas la 23), ou **24 à 26**.
Pour vérifier, ouvre un terminal et tape :

```bash
node -v
```

Si la commande n'existe pas, installe Node.js depuis https://nodejs.org (choisis
la version "LTS").

## 2. Installer

Une seule commande :

```bash
npm install -g omniroute
```

Le `-g` veut dire "globalement" : la commande `omniroute` devient disponible
partout sur ta machine. L'installation télécharge environ 1160 paquets, compte
2 à 3 minutes.

Des messages en jaune (`npm warn`) peuvent défiler : c'est normal, ce sont des
avertissements, pas des erreurs.

## 3. Vérifier que c'est bien installé

```bash
omniroute --version
```

Ça doit afficher un numéro, par exemple `3.8.50`.

## 4. Démarrer

```bash
omniroute serve
```

Le démarrage prend quelques secondes, puis un logo s'affiche avec deux adresses :

- le tableau de bord, à ouvrir dans ton navigateur : **http://localhost:20128**
- l'API, à donner à tes outils : **http://localhost:20128/v1**

Pour changer de port :

```bash
omniroute serve --port 3000
```

## 5. Créer ton compte et récupérer ta clé

La passerelle démarre toute seule, mais elle n'est pas ouverte pour autant :
tant que tu n'as pas de compte, le tableau de bord te renvoie vers une page de
connexion et l'API répond `401 — Authentication required`.

Ouvre donc **http://localhost:20128** dans ton navigateur : OmniRoute te fera
créer un identifiant, et c'est là que tu obtiendras la clé d'API à donner à tes
outils.

Si tu oublies ton mot de passe :

```bash
omniroute-reset-password
```

## 6. N'écouter qu'en local (recommandé)

Par défaut, OmniRoute écoute sur **toutes** les interfaces réseau, pas seulement
sur ton ordinateur. Il le signale d'ailleurs lui-même au démarrage :

> ⚠ SECURITY: listening on 0.0.0.0 with NO API-key requirement — the inference
> plane (/v1/*) is reachable by ANY device that can route to this host, and
> requests are billed to your configured providers.

Autrement dit : sur un réseau que tu ne maîtrises pas (wifi public, réseau
d'entreprise), une autre machine pourrait envoyer des requêtes facturées sur tes
comptes. Pour n'autoriser que ta propre machine :

```bash
OMNIROUTE_SERVER_HOST=127.0.0.1 omniroute serve
```

Pour ne pas avoir à le retaper à chaque fois, inscris-le une bonne fois dans ton
fichier de réglages personnel :

```bash
echo "OMNIROUTE_SERVER_HOST=127.0.0.1" >> ~/.omniroute/.env
```

Ensuite un simple `omniroute serve` suffit. Tu sauras que c'est pris en compte
parce que l'avertissement orange ci-dessus ne s'affiche plus au démarrage.

**À propos de `~/.omniroute/.env`** : c'est le fichier de réglages personnels
d'OmniRoute, rangé dans ton dossier utilisateur. C'est le bon endroit pour tout
réglage durable, car il l'emporte sur les réglages livrés avec le programme.
Il contient aussi ta clé de chiffrement (`STORAGE_ENCRYPTION_KEY`, créée
automatiquement au premier démarrage) : ne le partage pas et ne le mets jamais
sur GitHub.

## 7. Les commandes utiles

Il y a environ 87 sous-commandes. Les principales :

| Commande | À quoi ça sert |
|---|---|
| `omniroute serve` | Démarrer la passerelle |
| `omniroute status` | Voir si elle tourne |
| `omniroute stop` | L'arrêter |
| `omniroute restart` | La redémarrer |
| `omniroute dashboard` | Ouvrir le tableau de bord dans le navigateur |
| `omniroute doctor` | Diagnostiquer les problèmes |
| `omniroute models` | Lister les modèles d'IA disponibles |
| `omniroute keys` | Gérer les clés d'API |
| `omniroute providers` | Gérer les fournisseurs d'IA |
| `omniroute cost` | Voir ce que ça coûte |
| `omniroute update` | Mettre à jour OmniRoute |
| `omniroute --help` | Afficher toute l'aide |

Il existe aussi des commandes de configuration automatique pour les outils de
développement : `omniroute setup-claude`, `setup-cursor`, `setup-codex`,
`setup-aider`, et une dizaine d'autres.

## 8. Désinstaller

```bash
npm uninstall -g omniroute
```
