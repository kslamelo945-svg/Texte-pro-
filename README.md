# TextePro 2.0

Version finale préparée comme application web mobile-first.

## Fonctionnalités

- Interface professionnelle responsive Samsung, iPhone, TECNO, itel et navigateurs modernes
- Correction
- Amélioration
- Organisation
- Photo depuis appareil photo ou galerie
- Photo -> lecture du texte -> traitement par IA
- Résultat copiable
- Historique local des 5 derniers résultats
- États de chargement et d'erreur
- Serveur sécurisé : les appels OpenAI restent côté serveur

## Architecture

**Client:** `public/index.html`
- UI et interactions
- sélection du mode
- photo via `<input type="file" accept="image/*" capture="environment">`
- appels uniquement vers `/api/text` et `/api/photo`

**Serveur:** `server.js`
- valide les requêtes
- utilise `OPENAI_API_KEY`
- appelle `openai.responses.create(...)`
- n'expose jamais la clé API au navigateur

## Installation

Node.js 20+ recommandé.

```bash
npm install
cp .env.example .env
```

Dans `.env`, le parent/tuteur qui gère le compte API ajoute sa clé :

```env
OPENAI_API_KEY=...
TEXT_MODEL=gpt-5.6-luna
PORT=3000
```

Puis :

```bash
npm start
```

Ouvrir `http://localhost:3000`.

## Mise en ligne

Déployer le serveur Node.js chez un hébergeur compatible et définir `OPENAI_API_KEY` dans les variables secrètes du serveur. Ne jamais mettre la clé dans `public/`, dans un fichier JavaScript client ou dans une application mobile distribuée.

Avant une ouverture publique, ajouter authentification, limites de requêtes, surveillance des coûts, HTTPS, stockage sécurisé et contrôles de contenu adaptés.

## Pourquoi GPT-5.6 Luna ?

Le catalogue OpenAI actuel décrit GPT-5.6 Luna comme un modèle GPT-5.6 optimisé pour les usages sensibles au coût et disponible via `v1/responses`. Le modèle peut recevoir du texte et des images en entrée. Pour une qualité supérieure, `TEXT_MODEL` peut être changé vers un modèle plus puissant compatible avec votre compte.

Référence officielle :
https://developers.openai.com/api/docs/models

## Ajustements futurs

- Modèle : `TEXT_MODEL` dans `.env`
- Prompt : `instruction()` et les `instructions` dans `server.js`
- Limite de longueur : `server.js`
- Photo : taille maximale JSON et traitement de l'image
- Historique : remplacer `localStorage` par une base de données si les comptes utilisateurs sont ajoutés

## Validation

1. Tester les trois modes sur 10 textes différents.
2. Tester une photo nette et une photo difficile à lire.
3. Vérifier qu'aucune clé API n'apparaît dans le navigateur.
4. Tester une requête sans texte et une requête trop longue.
5. Tester une panne API et vérifier le message d'erreur.
6. Vérifier sur Samsung, iPhone, TECNO et itel.
7. Mesurer les coûts avant d'ouvrir l'accès à beaucoup d'utilisateurs.

L'implémentation utilise `responses.create` et non les anciennes API Completions/Chat Completions.
