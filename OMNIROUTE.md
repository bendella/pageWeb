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

La passerelle démarre sur **http://localhost:20128** et l'API se trouve à
**http://localhost:20128/v1**. Elle fonctionne dès le départ, sans clé d'API,
grâce au modèle `auto` pré-configuré.

Pour changer de port :

```bash
omniroute serve --port 3000
```

## 5. Les commandes utiles

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

## 6. Désinstaller

```bash
npm uninstall -g omniroute
```
