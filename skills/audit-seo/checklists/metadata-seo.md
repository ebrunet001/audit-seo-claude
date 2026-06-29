# Checklist — Métadonnées

Référencé par `SKILL.md` § 4.2.

## `<title>`

- [ ] Présent sur **chaque** page
- [ ] **Unique** par page (pas le même title sur 100 pages)
- [ ] Longueur ~ 30–60 caractères (50–60 idéal en pixels)
- [ ] Mot-clé principal en début de title quand pertinent
- [ ] Nom de marque en fin (séparateur `|`, `–`, `—` cohérent sur tout le site)
- [ ] Cohérent avec `<h1>` mais pas identique mot pour mot
- [ ] Pas de bourrage de mots-clés (keyword stuffing)
- [ ] Pas de `undefined`, `null`, `[object Object]`, placeholders oubliés

**Où chercher** :
- Next.js : `metadata` exporté de `layout.tsx`/`page.tsx`, `<title>` dans `_document.tsx`
- Nuxt : `useHead`, `useSeoMeta`, `definePageMeta`
- Astro : `<title>` dans layouts, props passées
- SvelteKit : `<svelte:head>`, `+page.ts` load
- Remix : export `meta`
- Laravel/Symfony : Twig/Blade `@yield('title')`, `{% block title %}`
- WordPress : Yoast/Rank Math/SEOPress + `<title>` dans `header.php`

## Meta description

- [ ] Présente sur les pages stratégiques (home, catégories, fiches produits, articles)
- [ ] Unique par page
- [ ] Longueur ~ 120–160 caractères
- [ ] Rédigée pour le **clic** (CTA implicite), pas pour le ranking direct
- [ ] Reflète honnêtement le contenu de la page
- [ ] Pas de `<meta name="description" content="">` vide
- [ ] Pas de duplication massive (souvent symptôme d'un template mal câblé)

## Open Graph

- [ ] `og:title` — peut différer du `<title>`, optimisé pour le partage social
- [ ] `og:description` — rédigée pour le clic social
- [ ] `og:image` — **absolue**, ≥ 1200×630 px, < 8 MB, format JPG/PNG/WebP
- [ ] `og:image:width` / `og:image:height` renseignés
- [ ] `og:image:alt` pour l'accessibilité
- [ ] `og:url` — canonical absolue
- [ ] `og:type` — `website` pour pages génériques, `article` pour articles
- [ ] `og:site_name` — nom de marque
- [ ] `og:locale` — `fr_FR`, `en_US`, etc.

## Twitter Cards

- [ ] `twitter:card` — `summary_large_image` recommandé pour articles et fiches
- [ ] `twitter:title`, `twitter:description`, `twitter:image` (peuvent reprendre l'OG)
- [ ] `twitter:site` et `twitter:creator` (handles `@...`) si pertinent
- [ ] Image distincte si stratégie de partage différente

## Canonical

> Voir aussi `technical-seo.md` § Canonical URLs.

- [ ] Présente sur toutes les pages indexables
- [ ] **Absolue** (`https://...`), pas relative
- [ ] Cohérente avec `og:url` et URL réelle

## Viewport

- [ ] `<meta name="viewport" content="width=device-width, initial-scale=1">` présent
- [ ] Pas de `user-scalable=no` (anti-pattern accessibilité, signal négatif mobile-first)
- [ ] Pas de `maximum-scale=1` sauf raison documentée

## Charset

- [ ] `<meta charset="utf-8">` **première** balise du `<head>` (dans les 1024 premiers octets)
- [ ] Pas de double déclaration
- [ ] Cohérent avec l'encodage réel des fichiers

## Robots meta

- [ ] Absent ou `index, follow` sur les pages publiques
- [ ] `noindex, follow` sur recherche interne, paniers, login
- [ ] Cohérent avec `X-Robots-Tag` et `robots.txt`
- [ ] `max-image-preview:large` recommandé pour permettre les rich snippets visuels

## Cohérence inter-templates

- [ ] Le composant `<Head>`/`<SEO>`/équivalent est utilisé de façon **uniforme** sur tous les templates
- [ ] Les valeurs par défaut (`og:image` fallback, `og:site_name`) sont centralisées
- [ ] Pas de templates qui oublient title ou description
- [ ] Stratégie multilingue cohérente (chaque locale a son set complet)

## Pièges fréquents

- 🚨 Title et description hardcodés dans `_document.tsx` Next.js → écrasés par chaque page mais facile à oublier
- 🚨 `og:image` relative (`/og.png`) → invalide pour les crawlers sociaux, doit être absolue
- 🚨 Meta description générée automatiquement par troncature du contenu → souvent médiocre
- 🚨 WordPress : conflit Yoast + thème qui dupliquent les balises
- 🚨 SPA React/Vue sans SSR : metas modifiés en JS ne sont pas vus par tous les crawlers (Twitter, LinkedIn historiquement, parfois Google)
