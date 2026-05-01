# 📦 LOUVAT 2 — Suivi de Production

Application web mobile-first pour le suivi du **chiffre d'affaires journalier** de la Biscuiterie Louvat. Les données sont synchronisées en temps réel via Firebase — accessible depuis n'importe quel appareil.

---

## 🎯 Contexte du projet

Outil interne destiné aux employés de production pour saisir les opérations (découpe, emballage…) au fil de la journée, suivre la progression vers les objectifs de CA, et archiver les journées dans un historique consultable.

---

## ✨ Fonctionnalités

| Onglet | Description |
|---|---|
| 📦 **Saisie** | Saisie rapide des opérations (kg × prix), sous-total en temps réel, liste des saisies du jour |
| 📊 **Journée** | Récapitulatif par opération + indicateur de tendance vs moyenne des 5 derniers jours |
| 🗂 **Historique** | Toutes les journées clôturées, avec total cumulé, moyenne et meilleure journée |
| ⚙️ **Config** | Gestion des opérations (nom + prix), objectifs de CA par palier de couleur |

**Fonctionnalités clés :**
- 🔄 **Synchronisation Firebase** — données partagées entre tous les appareils en temps réel
- 🟢 **Point de statut** dans le header (syncing / synced / erreur)
- 📶 **Mode hors-ligne** — fallback automatique sur le localStorage si Firebase est inaccessible
- 🎨 **Carte CA colorée** — rouge / orange / vert / doré selon les objectifs configurés
- 📈 **Indicateur de tendance** — +/- % par rapport à la moyenne des 5 derniers jours
- 🗃️ **Résumé historique** — total cumulé, moyenne / jour, meilleure journée
- 💾 **Export CSV** — journée en cours ou historique complet
- 🔔 **Détection des saisies de la veille** — propose d'archiver automatiquement au démarrage
- ✅ **Modal de confirmation** sur toutes les actions destructives

---

## 🗂️ Structure du projet

```
louvat2/
└── index.html   # Fichier unique — HTML, CSS et JS intégrés
```

Projet **mono-fichier**, aucune dépendance à installer. S'ouvre directement dans un navigateur.

---

## 🔥 Configuration Firebase

L'application utilise **Firebase Realtime Database** pour persister et synchroniser les données.

```javascript
// En haut du bloc <script> dans index.html
var FB_URL = 'https://prodlouvat-default-rtdb.europe-west1.firebasedatabase.app/state.json';
```

### Règles Firebase recommandées

Dans la console Firebase → Realtime Database → Règles :

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

> ⚠️ Ces règles sont adaptées à un usage interne sur réseau de confiance.

### Structure des données dans Firebase

```json
{
  "entries":    [],
  "operations": [],
  "history":    [],
  "objectifs":  { "rouge": 4500, "orange": 5000, "vert": 10000 }
}
```

---

## 🚀 Utilisation

### Ouvrir localement
```bash
git clone https://github.com/votre-utilisateur/louvat2.git
open louvat2/index.html
```

### Déployer en ligne
- **GitHub Pages** — Settings → Pages → branche `main`
- **Netlify / Vercel** — glisser-déposer `index.html`

Une fois en ligne, partager l'URL suffit — toutes les données sont centralisées sur Firebase.

---

## 🛠️ Guide d'utilisation

### 1. Configurer les opérations (⚙️ Config)
Ajouter les opérations avec leur prix €/kg (obligatoire). Les opérations peuvent être modifiées (✎) ou supprimées (✕) sans affecter les saisies existantes.

### 2. Saisir une opération (📦 Saisie)
1. Choisir une opération → le prix se remplit automatiquement
2. Entrer la quantité en kg
3. Vérifier le sous-total → **✔ Ajouter**

### 3. Clôturer la journée
**💾 Clôturer et sauvegarder** en fin de journée. Les saisies sont archivées et le formulaire réinitialisé. Si des saisies de la veille sont détectées au chargement, l'app propose de les archiver automatiquement.

### 4. Historique (🗂 Historique)
Total cumulé, moyenne/jour, meilleure journée. Chaque journée est dépliable. Export CSV disponible.

---

## 🎨 Système de couleurs

| Couleur | Condition | Signification |
|---|---|---|
| 🔴 Rouge | CA < seuil rouge | En dessous de l'objectif |
| 🟠 Orange | seuil rouge ≤ CA < seuil orange | Presque là |
| 🟢 Vert | seuil orange ≤ CA ≤ seuil vert | Objectif atteint |
| ⭐ Doré | CA > seuil vert | Excellent — objectif dépassé |

Seuils configurables dans **⚙️ Config → Objectifs de CA**. L'ordre rouge < orange < vert est obligatoire.

---

## 🧰 Technologies

| Technologie | Usage |
|---|---|
| HTML5 / CSS3 | Structure et mise en page |
| JavaScript Vanilla | Toute la logique applicative |
| Firebase Realtime Database | Persistance et synchronisation temps réel |
| `fetch` API (async/await) | Communication REST avec Firebase |
| `localStorage` | Backup local + migration ancienne version |
| Google Fonts | Barlow Condensed + Barlow |

**Aucun framework, aucune bibliothèque externe — zéro dépendance.**

---

## 📄 Licence

Projet réalisé pour usage interne — Biscuiterie Louvat, 2026.
