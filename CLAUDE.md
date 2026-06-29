# تطبيق القرآن الكريم — Quran App

## Vue d'ensemble

Application web de lecture et de recherche dans le Coran.
Hébergée sur **GitHub Pages** — pas de build, pas de bundler, pas de dépendances npm.
Tout le CSS et le JS de l'application restent dans `index.html` (single-file) ; seules les
librairies tierces vendorées (jQuery Mobile) vivent dans `lib/`.

## Fichiers du projet

```
QuranAppHTML/
├── index.html                          ← L'application complète (CSS + JS inline, source unique de vérité)
├── lib/
│   └── jquery-mobile-1.4.5/            ← jQuery + jQuery Mobile vendorés (panel, collapsible, listview)
│       ├── jquery-1.11.1.min.js
│       ├── jquery.mobile-1.4.5.min.js
│       └── jquery.mobile-1.4.5.min.css
├── CLAUDE.md                           ← Ce fichier
├── README.md                           ← Documentation utilisateur
├── QuranApp.code-workspace
└── .claude/
    └── settings.json                   ← Permissions Claude Code
```

## Architecture technique

- **HTML quasi single-file** : tout le CSS et le JS applicatif sont dans `index.html` ; `lib/` ne contient que jQuery Mobile (vendoré, pas de npm/CDN)
- **jQuery Mobile 1.4.5** utilisé uniquement pour 3 widgets, initialisés manuellement en JS (`autoInitializePage = false`, pas de système de "pages"/hash de jQM — nos ancres `#surah-N` restent gérées par le navigateur) :
  - `#sidebar` → **panel** overlay (`data-display="overlay" data-position="right"`), actif seulement en mobile (≤768px) ; le bureau garde un sidebar fixe classique
  - `#surah-collapsible` et `#results-collapsible` → **collapsible** (icônes `carat-d`/`carat-u`)
  - `#index-container` → **listview** (`data-inset="false"`), stylé pour ressembler à de simples liens (pas de chevrons, pas de bordures jQM)
- **Polices** : Google Fonts — `Noto Kufi Arabic` (texte du Coran) + `Amiri` (noms de sourates)
- **Direction** : RTL (droite à gauche) pour tout le contenu arabe
- **Données** : Le texte intégral du Coran (6236 ayats, 114 sourates) est directement dans le HTML

## Thème

Palette claire "verdure" définie via variables CSS (`:root` dans `index.html`) :
`--bg-page`, `--bg-sidebar`, `--green-deep`, `--green-medium`, `--green-medium-2`, `--gold`, etc.
Toujours utiliser ces variables plutôt que des couleurs en dur pour rester cohérent.

## Structure du HTML (sidebar)

```
#app
├── #sidebar-toggle (☰, mobile uniquement)
├── #sidebar (panel jQuery Mobile sur mobile, fixe sur desktop)
│   ├── #sidebar-header — titre
│   ├── #surah-collapsible (collapsible, replié par défaut)
│   │   └── #index-container (ul, listview) — 114 × .idx-item
│   ├── .display-group "عرض الآيات" — radios continu/séquentiel
│   └── .display-group#search-row "بحث"
│       ├── #search-box / #search-btn
│       ├── #search-count
│       └── #results-collapsible (collapsible, auto-ouvert si résultats)
│           └── #search-results (table des versets trouvés, cliquable)
└── #main-content
    ├── #page-title
    ├── #selection-bar — badges .sel-badge (#words2 / #lettres2)
    └── #quran-content → .surah × 114 → .surah-header + .surah-text (.aya × 6236, id="aya-N")
```

## Fonctionnalités actuelles

- **Comptage en temps réel** : sélectionner du texte → `selectionchange` → mise à jour des badges `#words2`/`#lettres2` (sans espaces, chiffres, ni signes ﴿﴾ ـ)
- **Recherche** : bouton "ابحث" ou Entrée → highlight `.highlight` (jaune) dans le texte + liste de résultats cliquable (sourate, n° de verset local, début du verset) dans `#results-collapsible`
- **Navigation depuis un résultat** : `goToAya()` scrolle vers l'aya (`#aya-N`) et la surligne (`.aya-active`) jusqu'au prochain clic
- **Index des sourates** : collapsible + listview, scroll smooth + surlignage de la sourate active (IntersectionObserver)
- **Panel mobile** : ouverture par le bouton ☰, ou par swipe du bord droit vers la gauche (détection tactile maison, jQM ne gère que le swipe pour *fermer*)

## Conventions de code

- **CSS** : variables du thème dans `:root`, pas de couleurs en dur pour les nouveaux éléments
- **JS** : fonctions globales simples (`doSearch()`, `scrollToSurah()`, `updateCount()`, `goToAya()`), pas de modules
- **Nommage** : IDs en kebab-case, classes en BEM simplifié
- **RTL** : `direction: rtl` sur tous les conteneurs de texte arabe
- **jQuery Mobile** : ne jamais laisser `autoInitializePage` à `true` ni activer `hashListeningEnabled`/`pushStateEnabled` — ça casserait la navigation par ancre `#surah-N`. Toujours enhancer les widgets manuellement (`.panel()`, `.collapsible()`, `.listview()`)

## Ce qu'il ne faut PAS faire

- Ne pas extraire le texte du Coran dans un fichier séparé (JSON, JS) — le texte reste dans `index.html`
- Ne pas ajouter d'autre framework/bundler que jQuery Mobile (déjà vendoré pour le panel/collapsible/listview mobile)
- Ne pas modifier le texte coranique lui-même
- Ne pas utiliser `innerHTML` pour injecter du texte utilisateur (risque XSS dans la recherche)
- Ne pas réactiver le système de pages/hash de jQuery Mobile

## Améliorations possibles

- Navigation ayat par ayat (précédent/suivant)
- Mode nuit (dark theme toggle)
- Taille de police ajustable
- Recherche avec navigation entre résultats (flèches suivant/précédent)
- Partage d'un ayat (copier lien avec ancre `#aya-N`)
- PWA / offline support (Service Worker)
