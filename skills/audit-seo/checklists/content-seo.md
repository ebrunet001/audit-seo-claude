# Checklist — Contenu, structure HTML & maillage interne

Référencé par `SKILL.md` § 4.3 et § 4.6.

## Structure HTML

### Hiérarchie des titres
- [ ] **Un seul `<h1>`** par page (sauf pages d'agrégation très spécifiques)
- [ ] Le H1 décrit le sujet principal de la page (cohérent avec title et URL)
- [ ] Hiérarchie Hn logique : pas de saut H1 → H3 → H2
- [ ] Pas de Hn utilisé uniquement pour le style (sinon : `<div>` + classe CSS)
- [ ] Headers cohérents au sein d'un même template

### Sémantique HTML
- [ ] `<main>` unique englobant le contenu principal
- [ ] `<header>`, `<footer>`, `<nav>` correctement utilisés
- [ ] `<article>` pour les contenus autonomes (posts, fiches produits)
- [ ] `<section>` avec heading explicite (pas vide)
- [ ] `<aside>` pour le contenu connexe (sidebars)
- [ ] `<button>` pour les actions, `<a>` pour les navigations (pas de div cliquables)
- [ ] Liens externes : `rel="noopener noreferrer"` pour `target="_blank"`

### Attributs alt
- [ ] **Toutes** les images de contenu ont un `alt` descriptif
- [ ] Images décoratives → `alt=""` (pas d'alt manquant)
- [ ] Pas de `alt="image"`, `alt="photo"`, nom de fichier brut
- [ ] Alt en cohérence avec le contenu autour

### Accessibilité ayant impact SEO
- [ ] `<html lang="fr">` (ou autre) renseigné et exact
- [ ] Labels associés aux inputs (`<label for>` ou `aria-label`)
- [ ] Contrastes suffisants (WCAG AA minimum)
- [ ] Pas de contenu masqué pour le SEO uniquement (cloaking → pénalité)
- [ ] Navigation utilisable au clavier
- [ ] Skip-links pour les longues navigations

### Liens cassés ou suspects
- [ ] Pas de liens vers `#` ou `javascript:void(0)` utilisés pour autre chose qu'un fallback
- [ ] Pas de `href=""` vide
- [ ] Liens internes en URLs relatives ou absolues, mais cohérent
- [ ] Pas de liens vers staging, localhost, ou domaines morts

## Maillage interne

- [ ] Chaque page importante reçoit au moins 1 lien interne depuis une autre page
- [ ] Pages prioritaires (piliers) reçoivent davantage de liens (de pages thématiquement proches)
- [ ] Profondeur de clic ≤ 3 depuis la home pour les pages critiques
- [ ] Ancres descriptives (pas « cliquez ici », « en savoir plus » répétés)
- [ ] Mix d'ancres exact match / partial match / branded / contextuelles
- [ ] Pas de sur-optimisation (100 liens internes vers la même page avec le même mot-clé)
- [ ] Pas de routes orphelines (présentes dans le code mais sans aucun lien entrant)
- [ ] Breadcrumbs visibles sur pages profondes (cohérents avec `BreadcrumbList` JSON-LD)
- [ ] Liens « articles connexes » / « produits similaires » pertinents

## Slugs et URLs

- [ ] Slugs descriptifs, courts (3–5 mots idéal)
- [ ] Mots-clés présents quand pertinent, sans bourrage
- [ ] En minuscules, séparés par `-`
- [ ] Pas d'IDs numériques seuls (`/produit/12345`)
- [ ] Pas de stopwords inutiles (`le`, `la`, `et`, `de` peuvent être supprimés)
- [ ] Stables dans le temps (changer un slug nécessite une redirection 301)

## Duplication de contenu

- [ ] **Titles uniques** sur l'ensemble du site
- [ ] **Meta descriptions uniques**
- [ ] H1 uniques (avec marge sur templates similaires)
- [ ] Pas de pages avec contenu quasi-identique sans canonical pointant vers l'original
- [ ] Variantes (couleur, taille produit) gérées via canonical ou paramètres canoniques
- [ ] Pas de filtre/tri qui crée 1000 URLs avec mêmes produits sans `noindex` ou canonical

## Cannibalisation

- [ ] Pas deux pages qui ciblent **la même intention de recherche** avec le même mot-clé principal
- [ ] Stratégie claire : page pilier vs articles satellites
- [ ] Si cannibalisation détectée : fusionner, rediriger, ou différencier l'intention

## Thin content

- [ ] Pages de contenu : ≥ 300 mots utiles (indicatif, pas une règle absolue)
- [ ] Pages produits : description originale (pas la fiche fabricant copiée)
- [ ] Pages catégories : intro éditoriale en plus de la grille de produits
- [ ] Pages tags / archives auto-générées vides → `noindex` ou suppression

## Intention de recherche

- [ ] Chaque page cible une intention claire (informationnelle / navigationnelle / transactionnelle / commerciale)
- [ ] Le format de la page correspond à l'intention (article long form, comparatif, page produit, landing)
- [ ] Le contenu répond à la question implicite du mot-clé

## Cohérence langue / pays

- [ ] `<html lang>` correct
- [ ] Contenu réellement dans la langue déclarée (pas de mix EN/FR involontaire)
- [ ] Multi-langue : structure URL claire (`/fr/`, `/en/` ou sous-domaines)
- [ ] hreflang correctement implémenté (voir `technical-seo.md`)
- [ ] Cohérence devise / unités / formats de date selon le marché
- [ ] Pas de traduction automatique sans relecture humaine sur les pages stratégiques

## Pièges fréquents

- 🚨 Title et H1 strictement identiques (perte d'opportunité, pas grave mais sous-optimal)
- 🚨 H1 contenant uniquement le logo
- 🚨 Plusieurs H1 par template
- 🚨 Pages produits avec descriptions fabricant identiques (duplicate content massif)
- 🚨 Sliders / carousels avec contenu invisible non-crawlable
- 🚨 Contenu chargé uniquement après interaction (« voir plus » en JS sans lien réel)
- 🚨 Blog avec catégories ET tags qui créent des doublons (`/category/seo/` et `/tag/seo/`)
