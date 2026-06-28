# 📖 تطبيق القرآن الكريم

Application web de lecture et de recherche dans le Coran, hébergée sur GitHub Pages.

## Fonctionnalités

- 📚 **114 sourates** complètes avec index de navigation rapide
- 🔍 **Recherche** dans tout le texte du Coran avec mise en surbrillance
- 📊 **Comptage** de mots et de lettres sur la sélection (en temps réel)
- 🖋️ **Police Noto Kufi Arabic** pour un rendu optimal du texte coranique
- ↔️ Entièrement en **RTL** (arabe)

## Utilisation

### Navigation par sourate
Cliquer sur le nom d'une sourate dans l'index à droite pour y accéder directement.

### Comptage de mots/lettres
Sélectionner un passage du texte → les compteurs se mettent à jour instantanément dans la barre latérale et dans la barre supérieure.

### Recherche
Saisir un mot ou une expression dans le champ de recherche → cliquer sur **ابحث** (ou appuyer sur Entrée) → toutes les occurrences sont mises en surbrillance en jaune.

## Déploiement

Le projet est un fichier HTML unique (`index.html`). Aucune installation requise.

Pour héberger sur GitHub Pages :
1. Pousser le fichier sur un dépôt GitHub
2. Aller dans `Settings → Pages → Source: main branch / root`
3. L'application est disponible à `https://<username>.github.io/<repo>/`

## Développement local

Ouvrir `index.html` directement dans un navigateur, ou lancer un serveur local :

```bash
# Python
python3 -m http.server 8080

# Node.js
npx serve .
```

## Stack technique

- HTML5 / CSS3 / JavaScript Vanilla
- Google Fonts : [Noto Kufi Arabic](https://fonts.google.com/noto/specimen/Noto+Kufi+Arabic) + [Amiri](https://fonts.google.com/specimen/Amiri)
- Aucune dépendance npm, aucun build
