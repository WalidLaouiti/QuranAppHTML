# تطبيق القرآن الكريم — Quran App

## Vue d'ensemble

Application web **single-file** (HTML + CSS + JS tout-en-un) de lecture et de recherche dans le Coran.
Hébergée sur **GitHub Pages** — pas de build, pas de bundler, pas de dépendances npm.

## Fichiers du projet

```
quranapp/
├── index.html          ← L'application complète (source unique de vérité)
├── CLAUDE.md           ← Ce fichier
├── README.md           ← Documentation utilisateur
├── .gitignore
├── .vscode/
│   └── settings.json   ← Config VS Code recommandée
└── .claude/
    ├── settings.json   ← Permissions Claude Code
    └── commands/
        ├── search.md   ← Commande /search
        └── stats.md    ← Commande /stats
```

## Architecture technique

- **Single HTML file** : tout le CSS et le JS sont dans `index.html`
- **Pas de framework** : JavaScript vanilla, pas de React/Vue/Angular
- **Polices** : Google Fonts — `Noto Kufi Arabic` (texte du Coran) + `Amiri` (noms de sourates)
- **Direction** : RTL (droite à gauche) pour tout le contenu arabe
- **Données** : Le texte intégral du Coran (6236 ayats, 114 sourates) est directement dans le HTML

## Structure du HTML

```
#app
├── #sidebar (position: fixed, right)
│   ├── #sidebar-header
│   │   ├── h2 — titre
│   │   ├── .stats-row — compteurs #words / #lettres
│   │   ├── #search-row — champ + bouton ابحث
│   │   └── #search-count — résultats
│   └── #index-container — liste des 114 sourates (.idx-item)
└── #main-content (margin-right: 280px)
    ├── #page-title
    ├── #selection-bar — badge compteurs #words2 / #lettres2
    └── #quran-content
        └── .surah × 114
            ├── .surah-header (.surah-num + .surah-name)
            └── .surah-text — texte de la sourate
```

## Fonctionnalités actuelles

- **Comptage en temps réel** : sélectionner du texte → `selectionchange` → mise à jour immédiate des compteurs de mots et lettres (sans espaces, chiffres, ni signes ﴿﴾ ـ)
- **Recherche** : bouton "ابحث" ou touche Entrée → highlight `.highlight` (jaune) sur toutes les occurrences + scroll vers la première + compteur de résultats
- **Index des sourates** : 114 liens → scroll smooth + surlignage de la sourate active dans l'index (IntersectionObserver)

## Conventions de code

- **CSS** : variables nommées logiquement — couleur principale `#1e3a2f` (vert foncé), accent `#c9a84c` (or)
- **JS** : fonctions globales simples (`doSearch()`, `scrollToSurah()`, `updateCount()`), pas de modules
- **Nommage** : IDs en kebab-case (`#search-box`, `#search-count`), classes en BEM simplifié (`.surah-header`, `.surah-text`)
- **RTL** : `direction: rtl` sur tous les conteneurs de texte arabe

## Ce qu'il ne faut PAS faire

- Ne pas extraire le texte du Coran dans un fichier séparé (JSON, JS) — le projet reste single-file
- Ne pas ajouter de framework ou de bundler
- Ne pas modifier le texte coranique lui-même
- Ne pas utiliser `innerHTML` pour injecter du texte utilisateur (risque XSS dans la recherche)

## Améliorations possibles

- Navigation ayat par ayat (précédent/suivant)
- Mode nuit (dark theme toggle)
- Taille de police ajustable
- Recherche avec navigation entre résultats (flèches suivant/précédent)
- Partage d'un ayat (copier lien avec ancre `#surah-X`)
- PWA / offline support (Service Worker)
