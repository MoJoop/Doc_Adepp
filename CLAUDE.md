# CLAUDE.md — Contexte Projet : Doc_Adepp

> Fichier de contexte pour Claude AI. Lire intégralement avant toute génération de code.

---

## 1. Vue d'ensemble du Projet

**Nom** : Doc_Adepp  
**Dépôt** : https://github.com/MoJoop/Doc_Adepp.git  
**Déploiement** : GitHub Pages (`https://mojoop.github.io/Doc_Adepp/`)  
**Public cible** : Étudiants du Master ADEPP — Aide à la Décision et Évaluation des Politiques Publiques, Université Cheikh Anta Diop (UCAD), Dakar, Sénégal  
**Langue principale de l'interface** : Français  
**Auteur/Admin** : Mamadou Diop (Ingénieur des Travaux Statistiques, ANSD)

---

## 2. Objectifs Fonctionnels

L'application est une **plateforme collaborative étudiante** avec deux modules principaux :

### Module 1 — Gestion Documentaire (Explorateur de fichiers)
- Afficher une arborescence de dossiers correspondant aux modules du Master
- Permettre la **création de nouveaux dossiers** par les utilisateurs
- Permettre l'**upload de fichiers** (PDF, DOCX, PPTX, XLSX, images, etc.) dans les dossiers
- Permettre le **téléchargement** de fichiers individuels et de dossiers entiers (ZIP)
- Afficher les **métadonnées** : nom du fichier, taille, date d'upload, uploader
- Recherche de fichiers par nom / dossier
- Interface drag-and-drop pour l'upload

### Module 2 — Planning / Calendrier académique
- Calendrier mensuel interactif
- Clic sur une date → formulaire pour ajouter un événement
- Types d'événements avec **code couleur** :
  - 🔴 Devoir / Évaluation
  - 🟠 Rendu de travaux
  - 🟢 Cours / Séance
  - 🔵 Conférence / Intervenant
  - 🟣 Soutenance
  - ⚫ Autre / Divers
- Affichage de la liste des événements sur la vue calendrier (badges colorés)
- Vue liste des événements à venir (sidebar ou onglet)
- Possibilité de modifier/supprimer un événement

---

## 3. Architecture Technique

### Contrainte principale
L'application est **100% statique** (hébergée sur GitHub Pages). Pas de serveur backend propre.

### Stack Technologique Retenue

| Couche | Technologie | Justification |
|--------|------------|---------------|
| Framework | **Vanilla HTML/CSS/JS** ou **Vue 3 + Vite** | Simplicité de déploiement GitHub Pages |
| Styles | **Tailwind CSS** (CDN) ou CSS custom | Rapidité, responsive |
| Icônes | **Lucide Icons** ou **Heroicons** | Légères, modernes |
| Fonts | **Google Fonts** (ex: Plus Jakarta Sans + DM Mono) | Distinctif, lisible |
| Stockage fichiers | **GitHub API v3** (via Personal Access Token) | Persistance cross-device sur le repo |
| Stockage événements | **GitHub API v3** ou **localStorage + JSON** | Selon choix d'authentification |
| ZIP téléchargement | **JSZip** (CDN) | Compression dossiers côté client |
| Animations | **CSS transitions** + **@keyframes** | Performances |

### Décision Architecture Stockage

**Option A (Recommandée) — GitHub API** :
- Un fichier `data/events.json` dans le repo stocke les événements du calendrier
- Les fichiers uploadés sont commités dans les dossiers via GitHub Contents API
- Nécessite un Personal Access Token (PAT) → configurable dans une page admin/settings
- Avantage : persistance réelle, accessible à tous les étudiants

**Option B — LocalStorage fallback** :
- Si pas de PAT configuré, l'app fonctionne en mode local (données dans le navigateur)
- Avertissement affiché à l'utilisateur

**Implémentation recommandée** : démarrer avec Option B pour le MVP, puis intégrer Option A.

### Structure du Dépôt

```
Doc_Adepp/
├── index.html              # Point d'entrée unique (SPA)
├── assets/
│   ├── css/
│   │   └── style.css       # Styles globaux + design system
│   ├── js/
│   │   ├── app.js          # Logique principale
│   │   ├── fileManager.js  # Gestion des fichiers/dossiers
│   │   ├── calendar.js     # Logique du calendrier
│   │   ├── githubApi.js    # Wrapper GitHub API
│   │   └── storage.js      # Abstraction stockage (localStorage / GitHub)
│   └── icons/
├── data/
│   ├── folders.json        # Définition initiale des dossiers
│   └── events.json         # Événements du calendrier (si GitHub API)
├── CLAUDE.md               # Ce fichier
├── SKILL.md                # Guide développement
└── README.md
```

---

## 4. Dossiers Initiaux du Master ADEPP

Ces dossiers doivent être présents au chargement initial de l'application (vides) :

```json
[
  "Droit de la commande publique",
  "Econométrie Var Qual",
  "Econométrie variable quantitative",
  "Economie du développement",
  "Economie Publique",
  "Environnement ressources naturelles et développement",
  "Evaluation environnementale",
  "Evaluation qualitative",
  "GAR",
  "Genre et développement",
  "L'Afrique dans la mondialisation",
  "Maîtrise d'ouvrage",
  "Micro-Evaluation d'impact",
  "Microsimulation",
  "MS Project",
  "Passation des marchés",
  "Plan d'orientation économique et social",
  "Préparation de projet",
  "Prospective",
  "Recherche de Fonds et financement d'un projet",
  "SIG",
  "Techniques de Négociation"
]
```

---

## 5. Design System

### Identité Visuelle

**Thème** : Académique moderne — sobre, professionnel, avec une touche de chaleur africaine.  
**Palette** :

```css
:root {
  /* Primaire — Bleu UCAD / institutionnel */
  --color-primary: #1A3A6B;
  --color-primary-light: #2B5EA7;
  --color-primary-dark: #0F2240;

  /* Accent — Or/Ocre (référence couleurs sahéliennes) */
  --color-accent: #C9892E;
  --color-accent-light: #E8A84A;

  /* Neutres */
  --color-bg: #F7F8FC;
  --color-surface: #FFFFFF;
  --color-surface-2: #EEF1F8;
  --color-border: #D8DDED;
  --color-text: #1C2340;
  --color-text-muted: #6B7280;

  /* Événements calendrier */
  --color-event-devoir: #EF4444;
  --color-event-rendu: #F97316;
  --color-event-cours: #22C55E;
  --color-event-conference: #3B82F6;
  --color-event-soutenance: #A855F7;
  --color-event-autre: #6B7280;

  /* Typographie */
  --font-display: 'Plus Jakarta Sans', sans-serif;
  --font-body: 'Plus Jakarta Sans', sans-serif;
  --font-mono: 'DM Mono', monospace;

  /* Espacements */
  --radius-sm: 6px;
  --radius-md: 10px;
  --radius-lg: 16px;
  --radius-xl: 24px;

  /* Shadows */
  --shadow-sm: 0 1px 3px rgba(26, 58, 107, 0.08);
  --shadow-md: 0 4px 16px rgba(26, 58, 107, 0.12);
  --shadow-lg: 0 8px 32px rgba(26, 58, 107, 0.16);
}
```

### Typographie

- **Titres** : Plus Jakarta Sans, 700-800
- **Corps** : Plus Jakarta Sans, 400-500
- **Code/Mono** : DM Mono, 400

### Composants UI Clés

#### Navigation (Header + Sidebar)
```
┌─────────────────────────────────────────────────────┐
│  📚 Doc ADEPP          [Recherche...]    [⚙ Config] │
├──────────┬──────────────────────────────────────────┤
│  Sidebar │  Zone principale                         │
│          │                                          │
│ 📁 Docs  │  [Fil d'Ariane] Accueil > Dossier        │
│ 📅 Plan. │                                          │
│          │  [Contenu]                               │
└──────────┴──────────────────────────────────────────┘
```

#### Layout Responsive
- **Desktop** (>1024px) : sidebar fixe gauche (280px) + contenu principal
- **Tablet** (768-1024px) : sidebar collapsible
- **Mobile** (<768px) : navigation bas d'écran (bottom nav), full-width content

---

## 6. Spécifications UX par Fonctionnalité

### 6.1 Explorateur de Fichiers

#### États de la vue dossier
1. **Vue grille** (défaut) : cartes avec icône dossier + nom + nombre de fichiers
2. **Vue liste** : tableau avec colonnes (nom, type, taille, date, uploader)
3. **Toggle** entre les deux vues

#### Upload de fichier
- Zone drop "Glisser-déposer ou cliquer"
- Indicateur de progression (barre + %)
- Validation côté client : taille max 25 MB, types acceptés configurables
- Confirmation après upload réussi (toast notification)

#### Actions sur fichier (clic droit ou menu contextuel)
- Télécharger
- Renommer
- Déplacer
- Supprimer (confirmation)

#### Téléchargement de dossier
- Utiliser JSZip pour créer un ZIP côté client
- Afficher le spinner pendant la compression

### 6.2 Calendrier

#### Vue Mensuelle
- Grille 7 colonnes × 5-6 lignes
- Jours avec événements : badges colorés (max 3 affichés + "X autres")
- Jour actuel mis en évidence
- Navigation mois précédent / suivant

#### Modal d'ajout d'événement (clic sur date)
Champs :
- Titre (requis)
- Type (select avec codes couleur)
- Date début / Date fin (avec heure optionnelle)
- Description (textarea)
- Matière associée (select parmi les dossiers existants)

#### Sidebar événements à venir
- Liste des 10 prochains événements
- Filtrables par type

---

## 7. Gestion de l'État (State Management)

```javascript
// Structure de l'état global de l'application
const appState = {
  // Navigation
  currentView: 'files' | 'calendar',
  currentFolder: null | string,

  // Gestionnaire de fichiers
  folders: [
    {
      id: 'uuid',
      name: 'string',
      parentId: null | 'uuid',  // null = racine
      createdAt: 'ISO date',
      createdBy: 'string',
    }
  ],
  files: [
    {
      id: 'uuid',
      name: 'string',
      folderId: 'uuid',
      size: number,  // bytes
      type: 'string',  // MIME type
      uploadedAt: 'ISO date',
      uploadedBy: 'string',
      url: 'string',  // URL de téléchargement
    }
  ],

  // Calendrier
  events: [
    {
      id: 'uuid',
      title: 'string',
      type: 'devoir' | 'rendu' | 'cours' | 'conference' | 'soutenance' | 'autre',
      startDate: 'ISO date',
      endDate: 'ISO date',
      description: 'string',
      folderId: null | 'uuid',  // Matière associée
      createdBy: 'string',
      createdAt: 'ISO date',
    }
  ],

  // UI
  searchQuery: 'string',
  viewMode: 'grid' | 'list',
  isLoading: boolean,
  notifications: [],

  // Config
  githubConfig: {
    token: 'string',  // Stocké en localStorage, jamais en dur
    owner: 'MoJoop',
    repo: 'Doc_Adepp',
    branch: 'main',
  }
};
```

---

## 8. GitHub API — Intégration

### Endpoints utilisés

```javascript
const GITHUB_API = 'https://api.github.com';
const REPO = 'MoJoop/Doc_Adepp';

// Lister le contenu d'un dossier
GET /repos/{REPO}/contents/{path}

// Créer/modifier un fichier
PUT /repos/{REPO}/contents/{path}
Body: { message, content (base64), sha (si update) }

// Supprimer un fichier
DELETE /repos/{REPO}/contents/{path}
Body: { message, sha }

// Lire un fichier JSON de données
GET /repos/{REPO}/contents/data/events.json
```

### Module githubApi.js

```javascript
class GitHubAPI {
  constructor(token) { this.token = token; }

  async getContents(path) { ... }
  async uploadFile(path, content, message) { ... }
  async deleteFile(path, sha, message) { ... }
  async updateJsonData(dataFile, newData) { ... }
}
```

---

## 9. Accessibilité & Performance

- Tous les éléments interactifs ont un `aria-label` ou `aria-labelledby`
- Navigation clavier complète (Tab, Enter, Escape pour modals)
- Score Lighthouse cible : ≥ 90 Performance, ≥ 95 Accessibility
- Images en lazy loading
- Fonts en `font-display: swap`
- Pas de JS bloquant (tout en `defer` ou `type="module"`)

---

## 10. Internationalisation

- Interface en **Français** uniquement pour le MVP
- Dates affichées au format **DD/MM/YYYY** (locale fr-FR)
- Nombres : locale `fr-FR` (ex: 1 234,56)

---

## 11. Sécurité

- Le Personal Access Token GitHub n'est **jamais** commité dans le code
- Il est stocké uniquement dans `localStorage` après saisie par l'utilisateur dans les settings
- Pas de système d'authentification utilisateur pour le MVP (accès ouvert)
- Validation côté client de tous les inputs (longueur, caractères interdits, MIME types)
- Sanitisation des noms de fichiers/dossiers (supprimer caractères spéciaux dangereux)

---

## 12. Workflow de Développement

### Ordre de développement recommandé

1. **Phase 1 — Structure & Design System** (index.html + CSS variables + layout)
2. **Phase 2 — Explorateur de fichiers** (localStorage mode, sans GitHub API)
3. **Phase 3 — Calendrier** (vue mensuelle + ajout d'événements)
4. **Phase 4 — GitHub API** (persistance réelle + upload fichiers)
5. **Phase 5 — Polish** (animations, transitions, responsive, a11y)

### Commandes Clés

```bash
# Développement local
npx serve .  # ou python3 -m http.server 8080

# Déploiement GitHub Pages (branche main, dossier root)
git add . && git commit -m "feat: ..." && git push origin main
```

### Conventions de Commits

```
feat: nouvelle fonctionnalité
fix: correction de bug
style: changements CSS/design
refactor: refactorisation sans changement de comportement
docs: documentation
chore: configuration, dépendances
```

---

## 13. Checklist MVP (Minimum Viable Product)

- [ ] Layout responsive avec navigation (sidebar desktop, bottom nav mobile)
- [ ] Affichage des 22 dossiers initiaux en vue grille et liste
- [ ] Création de nouveaux dossiers (modal + validation)
- [ ] Upload de fichiers avec drag & drop
- [ ] Téléchargement de fichiers
- [ ] Téléchargement de dossier en ZIP
- [ ] Calendrier mensuel avec navigation
- [ ] Ajout d'événement avec type et couleur
- [ ] Affichage des événements sur le calendrier
- [ ] Recherche de fichiers
- [ ] Notifications toast (succès, erreur, info)
- [ ] Mode sombre (toggle)
- [ ] Page de configuration GitHub API (optionnelle)

---

## 14. Notes Importantes pour Claude

1. **Toujours générer du code fonctionnel** — pas de placeholders `// TODO`
2. **Commenter en français** les parties complexes du code
3. **Tester mentalement** chaque composant avant de le coder (edge cases)
4. **Référencer ce fichier** au début de chaque session de développement
5. **Lire SKILL.md** avant de générer du code pour respecter les bonnes pratiques
6. L'app doit fonctionner **sans backend** en mode standalone avec localStorage
7. Utiliser des **UUID v4** (via `crypto.randomUUID()`) pour les IDs
8. **Pas de dépendances npm** dans le MVP — tout via CDN pour compatibilité GitHub Pages simple
9. Le code doit être **dans un seul fichier HTML** pour la première version, puis refactorisé
