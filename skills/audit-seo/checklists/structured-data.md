# Checklist — Données structurées (JSON-LD / Schema.org)

Référencé par `SKILL.md` § 4.4.

## Format & placement

- [ ] **JSON-LD** privilégié (pas Microdata ni RDFa, sauf legacy)
- [ ] Placé dans `<head>` ou en début de `<body>`
- [ ] `<script type="application/ld+json">` (pas `text/javascript`)
- [ ] JSON valide (pas de virgules traînantes, chaînes échappées correctement)
- [ ] Un script par type principal, ou un `@graph` regroupant plusieurs entités

## Types à couvrir selon le site

### Tous les sites
- [ ] `Organization` ou `LocalBusiness` (logo, nom, URL, sameAs vers réseaux sociaux)
- [ ] `WebSite` avec `potentialAction` `SearchAction` si recherche interne

### Sites éditoriaux / blog
- [ ] `Article` (ou `NewsArticle`, `BlogPosting`) sur chaque article
  - `headline`, `image`, `datePublished`, `dateModified`, `author`, `publisher`
- [ ] **`author` est un objet `Person` étendu (E-E-A-T)**, pas une simple string. Le pattern 2026 (depuis l'emphasis E-E-A-T de Google et l'émergence des AI Overviews) recommande :
  - `@type: Person` avec `name`
  - `url` pointant vers une page auteur stable (bio individuelle)
  - `image` (photo de l'auteur)
  - `jobTitle` (rôle / titre)
  - `worksFor: { @type: Organization, name, url }`
  - `sameAs: [...]` avec liens vers profils sociaux et académiques (LinkedIn, ORCID, Wikipedia si applicable, profils universitaires) — signal d'authoritativeness fort pour les LLMs
  - `knowsAbout: [...]` (sujets de compétence) pour les contenus techniques
- [ ] **`reviewedBy`** sur les contenus YMYL (Your Money Your Life — santé, finance, droit, science) : `reviewedBy: { @type: Person, name, jobTitle, ... }` avec un expert distinct de l'auteur. Signal d'authoritativeness amplifié.
- [ ] `BreadcrumbList` sur pages profondes

### E-commerce
- [ ] `Product` sur chaque fiche
  - `name`, `image`, `description`, `sku`, `brand`, `offers` (avec `price`, `priceCurrency`, `availability`)
- [ ] `AggregateRating` et `Review` si avis présents
- [ ] `BreadcrumbList`

### FAQ / aide / support
- [ ] `FAQPage` uniquement si la FAQ est **visible sur la page** (Google a restreint)
- [ ] `HowTo` pour les tutoriels étape par étape

### Vidéo
- [ ] `VideoObject` avec `name`, `description`, `thumbnailUrl`, `uploadDate`, `duration`

### Événements
- [ ] `Event` avec `startDate`, `location`, `offers`

### Recettes, films, livres, jobs…
- [ ] Types Schema.org correspondants

## Cohérence

- [ ] Les données JSON-LD **correspondent au contenu visible** (sinon = spam structuré)
- [ ] `image` est une URL absolue, accessible publiquement
- [ ] `url` est canonique
- [ ] Les dates sont au format ISO 8601 (`2025-03-14T10:30:00+01:00`)
- [ ] Les prix incluent la devise (`priceCurrency: "EUR"`)
- [ ] `Organization` global est identique sur toutes les pages (centralisé dans le layout)

## Validation

- [ ] Tester via **Rich Results Test** : https://search.google.com/test/rich-results
- [ ] Tester via **Schema Markup Validator** : https://validator.schema.org/
- [ ] Vérifier dans Google Search Console → Rich Results / Améliorations
- [ ] Aucune erreur bloquante ; les warnings sont acceptables si justifiés

## Pièges fréquents

- 🚨 JSON-LD copié-collé d'un tutoriel sans adaptation → données génériques fausses
- 🚨 `FAQPage` avec questions non visibles à l'utilisateur → pénalité manuelle Google
- 🚨 `Product` avec `price: "0"` ou `availability` incorrect
- 🚨 `Article` sans `author` ou avec `author` en simple string au lieu d'un objet `Person`/`Organization`
- 🚨 `author: { @type: Person, name: "Jean Dupont" }` minimal sans `url` / `sameAs` / `jobTitle` / `worksFor` — manque le signal E-E-A-T attendu en 2026. Google et les LLMs s'appuient sur ces champs pour évaluer l'authoritativeness; l'absence laisse de la performance SEO sur la table, particulièrement sur les contenus YMYL.
- 🚨 Plusieurs `Organization` différents sur le même site
- 🚨 `BreadcrumbList` qui ne reflète pas l'arborescence réelle
- 🚨 Données structurées générées en JS côté client → parfois ratées par les crawlers

## Où chercher dans le code

- Next.js : composants `<JsonLd>`, `next-seo`, `<Script type="application/ld+json">` dans `app/layout.tsx`
- Nuxt : `useHead` avec `script: [{ type: 'application/ld+json', innerHTML: ... }]`, module `nuxt-jsonld`
- Astro : `<script type="application/ld+json" set:html={...}>` dans layout
- WordPress : Yoast/Rank Math génèrent JSON-LD automatiquement → vérifier conflits avec ajouts manuels
- SvelteKit : `<svelte:head>` avec balise script

## Exemple minimal `Organization` + `WebSite`

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@graph": [
    {
      "@type": "Organization",
      "@id": "https://example.com/#organization",
      "name": "Example",
      "url": "https://example.com",
      "logo": "https://example.com/logo.png",
      "sameAs": [
        "https://twitter.com/example",
        "https://www.linkedin.com/company/example"
      ]
    },
    {
      "@type": "WebSite",
      "@id": "https://example.com/#website",
      "url": "https://example.com",
      "name": "Example",
      "publisher": { "@id": "https://example.com/#organization" },
      "potentialAction": {
        "@type": "SearchAction",
        "target": "https://example.com/search?q={search_term_string}",
        "query-input": "required name=search_term_string"
      }
    }
  ]
}
</script>
```
