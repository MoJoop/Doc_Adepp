---
name: doc-adepp-web-development
description: >
  Guide des meilleures pratiques pour le développement du projet Doc_Adepp.
  Application web statique (GitHub Pages) pour les étudiants du Master ADEPP
  de l'UCAD. Stack : HTML/CSS/JS Vanilla + optionnellement Vue 3. Design system
  académique premium, responsive, accessible. Lire avant toute génération de code.
---

# SKILL.md — Meilleures Pratiques Développement Web
## Projet Doc_Adepp — Master ADEPP, UCAD Dakar

---

## 1. Principes Fondamentaux

### Règle d'or : Fonctionnel avant Parfait
Toujours livrer du code **qui fonctionne réellement**. Pas de TODO, pas de placeholder.
Chaque composant généré doit être testable immédiatement dans un navigateur.

### Priorités par ordre décroissant
1. **Fonctionnalité** — ça marche
2. **Accessibilité** — tout le monde peut l'utiliser
3. **Performance** — ça charge vite
4. **Maintenabilité** — ça se comprend
5. **Esthétique** — ça est beau

### Contrainte GitHub Pages (critique)
- Pas de backend, pas de Node.js en production
- Tout en **JS vanilla** ou **Vue 3 CDN**
- Pas de `require()` — utiliser les **ES Modules** (`import/export`)
- Compatibilité : dernières versions Chrome, Firefox, Safari, Edge

---

## 2. Structure HTML — Conventions

### Document de base
```html
<!DOCTYPE html>
<html lang="fr" data-theme="light">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta name="description" content="Plateforme documentaire Master ADEPP — UCAD" />
  <title>Doc ADEPP</title>

  <!-- Fonts — preconnect d'abord pour performance -->
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=DM+Mono&display=swap" rel="stylesheet" />

  <!-- Styles critiques inline pour éviter FOUC -->
  <style>/* critical CSS ici */</style>

  <!-- Styles principaux -->
  <link rel="stylesheet" href="assets/css/style.css" />
</head>
<body>
  <!-- Contenu ici -->

  <!-- Scripts en bas, type module -->
  <script type="module" src="assets/js/app.js"></script>
</body>
</html>
```

### Sémantique HTML5 obligatoire
```html
<!-- ✅ Toujours utiliser les éléments sémantiques corrects -->
<header role="banner">...</header>
<nav aria-label="Navigation principale">...</nav>
<main id="main-content">...</main>
<aside aria-label="Panneau latéral">...</aside>
<footer>...</footer>

<!-- ✅ Sections avec landmarks -->
<section aria-labelledby="section-title">
  <h2 id="section-title">Titre de section</h2>
  ...
</section>

<!-- ❌ Jamais de divs pour tout -->
<div class="nav">...</div>  <!-- INTERDIT -->
```

---

## 3. CSS — Design System et Conventions

### Variables CSS — Définition complète requise
```css
/* Toujours déclarer toutes les variables dans :root */
:root {
  /* Couleurs primaires */
  --color-primary: #1A3A6B;
  --color-primary-light: #2B5EA7;
  --color-primary-dark: #0F2240;
  --color-accent: #C9892E;
  --color-accent-light: #E8A84A;

  /* Surfaces */
  --color-bg: #F7F8FC;
  --color-surface: #FFFFFF;
  --color-surface-2: #EEF1F8;
  --color-surface-hover: #E4E9F5;
  --color-border: #D8DDED;
  --color-border-focus: var(--color-primary-light);

  /* Textes */
  --color-text: #1C2340;
  --color-text-muted: #6B7280;
  --color-text-inverse: #FFFFFF;

  /* Statuts */
  --color-success: #22C55E;
  --color-warning: #F97316;
  --color-error: #EF4444;
  --color-info: #3B82F6;

  /* Événements calendrier */
  --color-event-devoir: #EF4444;
  --color-event-rendu: #F97316;
  --color-event-cours: #22C55E;
  --color-event-conference: #3B82F6;
  --color-event-soutenance: #A855F7;
  --color-event-autre: #6B7280;

  /* Typographie */
  --font-base: 'Plus Jakarta Sans', system-ui, sans-serif;
  --font-mono: 'DM Mono', monospace;

  --text-xs: clamp(0.7rem, 0.7vw, 0.75rem);
  --text-sm: clamp(0.8rem, 0.9vw, 0.875rem);
  --text-base: clamp(0.9rem, 1vw, 1rem);
  --text-md: clamp(1rem, 1.2vw, 1.125rem);
  --text-lg: clamp(1.1rem, 1.5vw, 1.25rem);
  --text-xl: clamp(1.3rem, 2vw, 1.5rem);
  --text-2xl: clamp(1.5rem, 2.5vw, 2rem);
  --text-3xl: clamp(2rem, 3vw, 2.5rem);

  --weight-normal: 400;
  --weight-medium: 500;
  --weight-semibold: 600;
  --weight-bold: 700;
  --weight-extrabold: 800;

  /* Espacements (système 4px) */
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-5: 20px;
  --space-6: 24px;
  --space-8: 32px;
  --space-10: 40px;
  --space-12: 48px;
  --space-16: 64px;

  /* Bordures */
  --radius-sm: 6px;
  --radius-md: 10px;
  --radius-lg: 16px;
  --radius-xl: 24px;
  --radius-full: 9999px;

  /* Ombres */
  --shadow-xs: 0 1px 2px rgba(26, 58, 107, 0.06);
  --shadow-sm: 0 1px 3px rgba(26, 58, 107, 0.08), 0 1px 2px rgba(26, 58, 107, 0.04);
  --shadow-md: 0 4px 6px rgba(26, 58, 107, 0.07), 0 2px 4px rgba(26, 58, 107, 0.04);
  --shadow-lg: 0 10px 15px rgba(26, 58, 107, 0.10), 0 4px 6px rgba(26, 58, 107, 0.05);
  --shadow-xl: 0 20px 25px rgba(26, 58, 107, 0.12), 0 10px 10px rgba(26, 58, 107, 0.04);

  /* Transitions */
  --transition-fast: 150ms ease;
  --transition-base: 250ms ease;
  --transition-slow: 350ms ease;

  /* Layout */
  --sidebar-width: 280px;
  --header-height: 64px;
  --content-max-width: 1400px;
  --z-sidebar: 100;
  --z-header: 200;
  --z-modal: 300;
  --z-toast: 400;
  --z-tooltip: 500;
}

/* Mode sombre */
[data-theme="dark"] {
  --color-bg: #0F1629;
  --color-surface: #1A2540;
  --color-surface-2: #1E2D4E;
  --color-surface-hover: #243358;
  --color-border: #2D3D65;
  --color-text: #E8EDF5;
  --color-text-muted: #8899BB;
  --shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.3);
  --shadow-md: 0 4px 6px rgba(0, 0, 0, 0.4);
  --shadow-lg: 0 10px 15px rgba(0, 0, 0, 0.5);
}
```

### Reset CSS moderne
```css
/* Box model universel */
*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

/* Fluidité typographique */
html {
  font-size: 16px;
  scroll-behavior: smooth;
  -webkit-text-size-adjust: 100%;
}

body {
  font-family: var(--font-base);
  font-size: var(--text-base);
  color: var(--color-text);
  background-color: var(--color-bg);
  line-height: 1.6;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* Images responsives */
img, video, canvas, svg {
  display: block;
  max-width: 100%;
}

/* Formulaires cohérents */
input, button, textarea, select {
  font: inherit;
}

/* Paragraphes */
p, h1, h2, h3, h4, h5, h6 {
  overflow-wrap: break-word;
}
```

### Layout Principal — Grid CSS
```css
.app-layout {
  display: grid;
  grid-template-areas:
    "header header"
    "sidebar main";
  grid-template-columns: var(--sidebar-width) 1fr;
  grid-template-rows: var(--header-height) 1fr;
  min-height: 100vh;
}

.app-header { grid-area: header; }
.app-sidebar { grid-area: sidebar; }
.app-main { grid-area: main; }

/* Responsive — Tablet */
@media (max-width: 1024px) {
  .app-layout {
    grid-template-columns: 0 1fr;
  }
  .app-sidebar {
    position: fixed;
    left: -100%;
    transition: left var(--transition-base);
    z-index: var(--z-sidebar);
  }
  .app-sidebar.is-open {
    left: 0;
  }
}

/* Responsive — Mobile */
@media (max-width: 768px) {
  .app-layout {
    grid-template-areas:
      "header"
      "main"
      "nav";
    grid-template-columns: 1fr;
    grid-template-rows: var(--header-height) 1fr 56px;
  }
  .app-sidebar { display: none; }
  .bottom-nav { display: flex; }
}
```

### Composants CSS — Patterns essentiels

#### Bouton
```css
.btn {
  display: inline-flex;
  align-items: center;
  gap: var(--space-2);
  padding: var(--space-2) var(--space-4);
  border: 1.5px solid transparent;
  border-radius: var(--radius-md);
  font-size: var(--text-sm);
  font-weight: var(--weight-semibold);
  cursor: pointer;
  transition: all var(--transition-fast);
  white-space: nowrap;
  user-select: none;
}

.btn:focus-visible {
  outline: 2px solid var(--color-border-focus);
  outline-offset: 2px;
}

.btn-primary {
  background: var(--color-primary);
  color: var(--color-text-inverse);
  border-color: var(--color-primary);
}
.btn-primary:hover {
  background: var(--color-primary-light);
  transform: translateY(-1px);
  box-shadow: var(--shadow-md);
}

.btn-ghost {
  background: transparent;
  color: var(--color-text-muted);
}
.btn-ghost:hover {
  background: var(--color-surface-hover);
  color: var(--color-text);
}
```

#### Card de Dossier
```css
.folder-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-6);
  background: var(--color-surface);
  border: 1.5px solid var(--color-border);
  border-radius: var(--radius-lg);
  cursor: pointer;
  transition: all var(--transition-base);
  text-align: center;
  position: relative;
  overflow: hidden;
}

.folder-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 3px;
  background: linear-gradient(90deg, var(--color-primary), var(--color-accent));
  opacity: 0;
  transition: opacity var(--transition-fast);
}

.folder-card:hover {
  border-color: var(--color-primary-light);
  box-shadow: var(--shadow-lg);
  transform: translateY(-2px);
}

.folder-card:hover::before {
  opacity: 1;
}
```

#### Modal
```css
.modal-backdrop {
  position: fixed;
  inset: 0;
  background: rgba(15, 22, 41, 0.6);
  backdrop-filter: blur(4px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: var(--z-modal);
  padding: var(--space-4);
  opacity: 0;
  transition: opacity var(--transition-base);
}

.modal-backdrop.is-visible {
  opacity: 1;
}

.modal {
  background: var(--color-surface);
  border-radius: var(--radius-xl);
  box-shadow: var(--shadow-xl);
  width: 100%;
  max-width: 520px;
  max-height: 90vh;
  overflow-y: auto;
  transform: scale(0.95) translateY(8px);
  transition: transform var(--transition-base);
}

.modal-backdrop.is-visible .modal {
  transform: scale(1) translateY(0);
}
```

#### Toast / Notification
```css
.toast-container {
  position: fixed;
  bottom: var(--space-6);
  right: var(--space-6);
  display: flex;
  flex-direction: column;
  gap: var(--space-2);
  z-index: var(--z-toast);
}

.toast {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-3) var(--space-4);
  background: var(--color-surface);
  border-radius: var(--radius-md);
  box-shadow: var(--shadow-lg);
  border-left: 4px solid var(--color-info);
  min-width: 300px;
  animation: slideInRight 0.3s ease forwards;
}

@keyframes slideInRight {
  from { transform: translateX(100%); opacity: 0; }
  to   { transform: translateX(0);    opacity: 1; }
}

.toast.toast-success { border-color: var(--color-success); }
.toast.toast-error   { border-color: var(--color-error);   }
.toast.toast-warning { border-color: var(--color-warning); }
```

---

## 4. JavaScript — Patterns et Architecture

### Module Pattern (ES Modules)
```javascript
// Chaque fichier est un module ES — toujours utiliser import/export

// storage.js — Abstraction du stockage
export const Storage = {
  get(key, defaultValue = null) {
    try {
      const item = localStorage.getItem(key);
      return item ? JSON.parse(item) : defaultValue;
    } catch {
      return defaultValue;
    }
  },

  set(key, value) {
    try {
      localStorage.setItem(key, JSON.stringify(value));
      return true;
    } catch {
      console.error(`Storage.set(${key}) failed:`, e);
      return false;
    }
  },

  remove(key) {
    localStorage.removeItem(key);
  },
};
```

### Gestion d'état centralisée (sans framework)
```javascript
// store.js — Store réactif minimal
class Store {
  #state;
  #listeners = new Map();

  constructor(initialState) {
    this.#state = structuredClone(initialState);
  }

  getState() {
    return structuredClone(this.#state);
  }

  setState(updater) {
    const prevState = this.#state;
    this.#state = typeof updater === 'function'
      ? updater(structuredClone(prevState))
      : { ...prevState, ...updater };

    this.#notify(prevState, this.#state);
  }

  subscribe(key, listener) {
    if (!this.#listeners.has(key)) {
      this.#listeners.set(key, new Set());
    }
    this.#listeners.get(key).add(listener);

    // Retourner la fonction de désabonnement
    return () => this.#listeners.get(key)?.delete(listener);
  }

  #notify(prev, next) {
    this.#listeners.forEach((listeners, key) => {
      if (prev[key] !== next[key]) {
        listeners.forEach(fn => fn(next[key], prev[key]));
      }
    });
  }
}

export const store = new Store({
  currentView: 'files',
  currentFolder: null,
  folders: [],
  files: [],
  events: [],
  searchQuery: '',
  viewMode: 'grid',
  isLoading: false,
});
```

### Gestion des erreurs — Pattern try/catch uniforme
```javascript
// utils/async.js
export async function tryCatch(asyncFn, errorMessage = 'Une erreur est survenue') {
  try {
    const result = await asyncFn();
    return { data: result, error: null };
  } catch (err) {
    console.error(errorMessage, err);
    return { data: null, error: err.message ?? errorMessage };
  }
}

// Usage :
const { data: folders, error } = await tryCatch(
  () => githubApi.getContents('docs/'),
  'Impossible de charger les dossiers'
);

if (error) {
  showToast(error, 'error');
  return;
}
// utiliser folders...
```

### Composants DOM — Factory Pattern
```javascript
// components/FolderCard.js
export function createFolderCard(folder) {
  const card = document.createElement('article');
  card.className = 'folder-card';
  card.setAttribute('role', 'button');
  card.setAttribute('tabindex', '0');
  card.setAttribute('aria-label', `Ouvrir le dossier ${folder.name}`);
  card.dataset.folderId = folder.id;

  card.innerHTML = `
    <div class="folder-icon" aria-hidden="true">
      <svg>...</svg>
    </div>
    <div class="folder-info">
      <h3 class="folder-name">${escapeHtml(folder.name)}</h3>
      <span class="folder-count">
        ${folder.fileCount ?? 0} fichier${folder.fileCount !== 1 ? 's' : ''}
      </span>
    </div>
    <button class="folder-menu-btn" aria-label="Options du dossier ${escapeHtml(folder.name)}">
      ...
    </button>
  `;

  // Événements
  card.addEventListener('click', (e) => {
    if (!e.target.closest('.folder-menu-btn')) {
      openFolder(folder.id);
    }
  });

  card.addEventListener('keydown', (e) => {
    if (e.key === 'Enter' || e.key === ' ') {
      e.preventDefault();
      openFolder(folder.id);
    }
  });

  return card;
}

// TOUJOURS échapper le HTML utilisateur
function escapeHtml(str) {
  const div = document.createElement('div');
  div.textContent = str;
  return div.innerHTML;
}
```

### Upload de fichiers — avec progression
```javascript
// fileManager.js
export async function uploadFile(file, folderId) {
  // Validation
  const MAX_SIZE = 25 * 1024 * 1024; // 25 MB
  if (file.size > MAX_SIZE) {
    throw new Error(`Le fichier dépasse 25 MB (${formatSize(file.size)})`);
  }

  // Convertir en base64
  const base64 = await fileToBase64(file);

  // Upload via GitHub API
  const path = `docs/${getFolderPath(folderId)}/${sanitizeFilename(file.name)}`;
  await githubApi.uploadFile(path, base64, `Upload: ${file.name}`);

  return {
    id: crypto.randomUUID(),
    name: file.name,
    folderId,
    size: file.size,
    type: file.type,
    uploadedAt: new Date().toISOString(),
    uploadedBy: 'Étudiant',
    path,
  };
}

function fileToBase64(file) {
  return new Promise((resolve, reject) => {
    const reader = new FileReader();
    reader.onload = () => resolve(reader.result.split(',')[1]);
    reader.onerror = reject;
    reader.readAsDataURL(file);
  });
}

function sanitizeFilename(name) {
  return name
    .normalize('NFD')
    .replace(/[\u0300-\u036f]/g, '')  // Supprimer accents
    .replace(/[^a-zA-Z0-9._-]/g, '_')  // Caractères sûrs uniquement
    .replace(/_+/g, '_');
}

export function formatSize(bytes) {
  if (bytes === 0) return '0 B';
  const k = 1024;
  const sizes = ['B', 'KB', 'MB', 'GB'];
  const i = Math.floor(Math.log(bytes) / Math.log(k));
  return `${parseFloat((bytes / Math.pow(k, i)).toFixed(1))} ${sizes[i]}`;
}
```

### Calendrier — Logique de rendu
```javascript
// calendar.js
export function renderCalendar(year, month, events) {
  const firstDay = new Date(year, month, 1).getDay();
  const daysInMonth = new Date(year, month + 1, 0).getDate();
  const today = new Date();

  // Ajuster pour commencer la semaine lundi (fr)
  const startOffset = (firstDay + 6) % 7;

  const cells = [];

  // Cellules vides de début
  for (let i = 0; i < startOffset; i++) {
    cells.push({ empty: true });
  }

  // Jours du mois
  for (let day = 1; day <= daysInMonth; day++) {
    const date = new Date(year, month, day);
    const dateStr = date.toISOString().split('T')[0];
    const dayEvents = events.filter(e =>
      e.startDate.startsWith(dateStr)
    );

    cells.push({
      day,
      date,
      dateStr,
      isToday: date.toDateString() === today.toDateString(),
      isPast: date < today,
      events: dayEvents,
    });
  }

  return cells;
}

export const EVENT_TYPES = {
  devoir:      { label: 'Devoir / Évaluation', color: '#EF4444', icon: '📝' },
  rendu:       { label: 'Rendu de travaux',    color: '#F97316', icon: '📬' },
  cours:       { label: 'Cours / Séance',      color: '#22C55E', icon: '📚' },
  conference:  { label: 'Conférence',          color: '#3B82F6', icon: '🎤' },
  soutenance:  { label: 'Soutenance',          color: '#A855F7', icon: '🎓' },
  autre:       { label: 'Autre',               color: '#6B7280', icon: '📌' },
};
```

---

## 5. Accessibilité — Checklist Obligatoire

### Focus visible en tout temps
```css
/* JAMAIS supprimer l'outline sans alternative */
:focus-visible {
  outline: 2px solid var(--color-primary-light);
  outline-offset: 2px;
  border-radius: var(--radius-sm);
}
/* Reset seulement pour la souris */
:focus:not(:focus-visible) {
  outline: none;
}
```

### Modals — gestion du focus
```javascript
class Modal {
  open(modalElement) {
    // Sauvegarder le focus actuel
    this._returnFocus = document.activeElement;

    // Afficher
    modalElement.removeAttribute('hidden');
    modalElement.setAttribute('aria-modal', 'true');

    // Déplacer le focus vers le premier élément focusable
    const firstFocusable = modalElement.querySelector(
      'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
    );
    firstFocusable?.focus();

    // Piéger le focus dans la modal
    modalElement.addEventListener('keydown', this._trapFocus);

    // Fermer sur Escape
    document.addEventListener('keydown', this._handleEscape);
  }

  close(modalElement) {
    modalElement.setAttribute('hidden', '');
    modalElement.removeAttribute('aria-modal');
    modalElement.removeEventListener('keydown', this._trapFocus);
    document.removeEventListener('keydown', this._handleEscape);

    // Restaurer le focus
    this._returnFocus?.focus();
  }

  _trapFocus = (e) => {
    if (e.key !== 'Tab') return;
    const focusable = e.currentTarget.querySelectorAll(
      'button:not([disabled]), [href], input:not([disabled]), select, textarea, [tabindex]:not([tabindex="-1"])'
    );
    const first = focusable[0];
    const last = focusable[focusable.length - 1];

    if (e.shiftKey && document.activeElement === first) {
      e.preventDefault();
      last.focus();
    } else if (!e.shiftKey && document.activeElement === last) {
      e.preventDefault();
      first.focus();
    }
  };
}
```

### ARIA sur les éléments dynamiques
```html
<!-- Zone de notification live -->
<div id="announcer" role="status" aria-live="polite" aria-atomic="true"
     class="sr-only"></div>

<!-- Annonces assistives -->
<script>
function announce(message) {
  const el = document.getElementById('announcer');
  el.textContent = '';
  requestAnimationFrame(() => { el.textContent = message; });
}
// Usage : announce('Dossier créé avec succès');
</script>

<!-- Classe utilitaire pour les éléments visibles seulement par lecteurs d'écran -->
<style>
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}
</style>
```

---

## 6. Performance — Règles Essentielles

### Debounce pour la recherche
```javascript
function debounce(fn, delay = 300) {
  let timeoutId;
  return (...args) => {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => fn(...args), delay);
  };
}

// Recherche avec debounce
const handleSearch = debounce((query) => {
  store.setState({ searchQuery: query });
  renderFilteredFiles(query);
}, 300);

searchInput.addEventListener('input', e => handleSearch(e.target.value));
```

### Virtualisation pour longues listes
```javascript
// Pour les listes > 100 items, n'afficher que le visible
function renderVirtualList(items, container, renderItem, itemHeight = 60) {
  const visibleCount = Math.ceil(container.clientHeight / itemHeight) + 5;
  let scrollTop = 0;

  function render() {
    const startIndex = Math.floor(scrollTop / itemHeight);
    const endIndex = Math.min(startIndex + visibleCount, items.length);

    container.style.position = 'relative';
    container.style.height = `${items.length * itemHeight}px`;

    // Nettoyer et re-render
    container.innerHTML = '';
    for (let i = startIndex; i < endIndex; i++) {
      const el = renderItem(items[i], i);
      el.style.position = 'absolute';
      el.style.top = `${i * itemHeight}px`;
      el.style.width = '100%';
      container.appendChild(el);
    }
  }

  container.addEventListener('scroll', () => {
    scrollTop = container.scrollTop;
    requestAnimationFrame(render);
  });

  render();
}
```

### Chargement différé des vues
```javascript
// Charger les modules uniquement quand nécessaire
async function loadCalendarView() {
  const { initCalendar } = await import('./calendar.js');
  initCalendar();
}

// Dans le routeur
async function navigate(view) {
  switch (view) {
    case 'calendar':
      await loadCalendarView();
      break;
    case 'files':
      await import('./fileManager.js').then(m => m.init());
      break;
  }
}
```

---

## 7. Animations — Lignes Directrices

### Animer uniquement les propriétés performantes
```css
/* ✅ Animer UNIQUEMENT ces propriétés (GPU-accélérées) */
transform: translate(), scale(), rotate();
opacity;
filter;

/* ❌ Ne jamais animer ces propriétés (reflow) */
width, height, top, left, margin, padding;
```

### Transitions d'entrée pour les cards
```css
.folder-card {
  animation: cardEnter 0.4s ease both;
}

/* Décalage en cascade avec nth-child */
.folder-card:nth-child(1)  { animation-delay: 0ms;  }
.folder-card:nth-child(2)  { animation-delay: 40ms; }
.folder-card:nth-child(3)  { animation-delay: 80ms; }
/* Générer en JS pour + de flexibilité */

@keyframes cardEnter {
  from {
    opacity: 0;
    transform: translateY(12px) scale(0.98);
  }
  to {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}
```

### Respecter la préférence utilisateur
```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

---

## 8. Sécurité — Règles Non Négociables

```javascript
// ✅ TOUJOURS échapper le HTML utilisateur
function escapeHtml(unsafe) {
  return unsafe
    .replace(/&/g, '&amp;')
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;')
    .replace(/"/g, '&quot;')
    .replace(/'/g, '&#039;');
}

// ✅ Valider les noms de fichiers/dossiers
function validateName(name) {
  if (!name || name.trim().length === 0) {
    return { valid: false, error: 'Le nom ne peut pas être vide' };
  }
  if (name.length > 100) {
    return { valid: false, error: 'Le nom ne peut pas dépasser 100 caractères' };
  }
  if (/[<>:"/\\|?*\x00-\x1f]/.test(name)) {
    return { valid: false, error: 'Le nom contient des caractères invalides' };
  }
  return { valid: true };
}

// ✅ Le token GitHub dans localStorage — JAMAIS dans le code source
const config = {
  get token() { return localStorage.getItem('github_pat'); },
  set token(val) { localStorage.setItem('github_pat', val); },
};

// ✅ MIME types autorisés pour l'upload
const ALLOWED_TYPES = [
  'application/pdf',
  'application/vnd.openxmlformats-officedocument.wordprocessingml.document',
  'application/vnd.openxmlformats-officedocument.presentationml.presentation',
  'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet',
  'application/msword',
  'image/jpeg', 'image/png', 'image/gif', 'image/webp',
  'text/plain', 'text/csv',
  'application/zip',
];

function validateFileType(file) {
  return ALLOWED_TYPES.includes(file.type);
}
```

---

## 9. Conventions de Nommage

### CSS — BEM Adapté
```css
/* Bloc */
.folder-card { }

/* Élément */
.folder-card__icon { }
.folder-card__name { }
.folder-card__count { }

/* Modificateur */
.folder-card--selected { }
.folder-card--loading { }
.folder-card--empty { }

/* États JS (préfixe is-/has-) */
.is-active { }
.is-loading { }
.has-error { }
.is-open { }
```

### JavaScript
```javascript
// Variables et fonctions : camelCase
const currentFolder = null;
function openFolderById(id) { }

// Constantes : SCREAMING_SNAKE_CASE
const MAX_FILE_SIZE = 25 * 1024 * 1024;
const DEFAULT_VIEW_MODE = 'grid';

// Classes : PascalCase
class GitHubAPI { }
class EventStore { }

// Handlers : préfixe handle
function handleUploadClick(e) { }
function handleFolderCreate(name) { }

// Booléens : préfixe is/has/can/should
const isLoading = true;
const hasFiles = files.length > 0;
const canDelete = userIsAdmin;

// IDs : toujours UUID
const id = crypto.randomUUID();
```

### HTML (data attributes pour le JS)
```html
<!-- ✅ Data attributes pour sélection JS, jamais les classes CSS -->
<div class="folder-card" data-folder-id="uuid" data-folder-name="...">

<!-- ❌ Ne pas cibler les classes CSS depuis JS pour la logique -->
document.querySelector('.folder-card')  // fragile
document.querySelector('[data-folder-id]')  // robuste ✅
```

---

## 10. Checklist Avant Chaque Commit

**Fonctionnel**
- [ ] Tous les cas d'usage principaux testés manuellement
- [ ] Pas d'erreurs dans la console
- [ ] Les erreurs sont gérées et affichées à l'utilisateur
- [ ] Le LocalStorage fallback fonctionne sans config GitHub

**Responsive**
- [ ] Vérifié sur mobile 375px
- [ ] Vérifié sur tablet 768px
- [ ] Vérifié sur desktop 1440px
- [ ] Pas de scroll horizontal indésirable

**Accessibilité**
- [ ] Navigation clavier testée (Tab, Shift+Tab, Enter, Escape)
- [ ] Labels sur tous les formulaires
- [ ] Focus visible sur tous les éléments interactifs
- [ ] Contraste texte ≥ 4.5:1 (WCAG AA)

**Performance**
- [ ] Pas de calculs lourds dans les event listeners (debounce si nécessaire)
- [ ] Pas de fuites mémoire (event listeners nettoyés)
- [ ] Images et assets optimisés

**Sécurité**
- [ ] Pas de secrets dans le code source
- [ ] HTML utilisateur échappé avant injection
- [ ] Fichiers uploadés validés (type + taille)

**Code Quality**
- [ ] Nommage expressif et cohérent
- [ ] Commentaires sur la logique complexe (en français)
- [ ] Pas de code mort ou commenté en production
