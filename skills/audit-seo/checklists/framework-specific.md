# Checklist — Pièges spécifiques par framework

Référencé par `SKILL.md` § 4.7.

## Next.js

### App Router (`app/`)
- [ ] `metadata` exporté depuis `layout.tsx` et `page.tsx` (pas `<Head>` legacy)
- [ ] `generateMetadata` async pour métas dynamiques basés sur la donnée
- [ ] `metadataBase` défini dans `layout.tsx` racine pour URLs absolues
- [ ] `app/robots.ts` et `app/sitemap.ts` utilisés (ou `public/robots.txt` mais pas les deux)
- [ ] `app/icon.tsx`, `app/apple-icon.tsx`, `app/opengraph-image.tsx` configurés
- [ ] Pas de `'use client'` sur les pages dont le contenu SEO doit être SSR
- [ ] `revalidate` ou `dynamic` choisi consciemment par route
- [ ] `next/image` avec `priority` sur le LCP, `loading="lazy"` ailleurs

### Pages Router (`pages/`)
- [ ] `<Head>` de `next/head` dans chaque page
- [ ] `_document.tsx` ne contient pas de `<title>` (à mettre par page)
- [ ] `getStaticProps` / `getServerSideProps` choisis selon volatilité
- [ ] `getStaticPaths` couvre les URLs critiques avec `fallback` adapté

### Pièges Next
- 🚨 Mélanger `app/` et `pages/` qui collisionnent
- 🚨 `images.unoptimized: true` en prod
- 🚨 `next/script` `strategy="beforeInteractive"` mal utilisé
- 🚨 Oublier `metadataBase` → `og:image` relatives en prod
- 🚨 `redirects()` dans `next.config.js` qui crée des chaînes

## Nuxt 3

- [ ] `useSeoMeta` ou `useHead` dans chaque `pages/*.vue`
- [ ] `definePageMeta` pour les options de page
- [ ] `nuxt.config.ts` → `app.head` pour les defaults globaux
- [ ] Module `@nuxtjs/seo` (ou équivalents) installé si site complexe
- [ ] `nuxt-simple-sitemap` ou `@nuxtjs/sitemap` pour sitemap
- [ ] `nuxt-simple-robots` pour robots.txt
- [ ] `ssr: true` (défaut) maintenu pour les pages publiques
- [ ] `payloadExtraction` activé pour les sites statiques

### Pièges Nuxt
- 🚨 `<ClientOnly>` enveloppant du contenu indexable
- 🚨 `useFetch` non-SSR sur du contenu critique
- 🚨 Oublier de générer le sitemap pour les routes dynamiques (`sitemap.urls` async)

## Astro

- [ ] Composant SEO centralisé (`<BaseLayout>` ou `<SEO>` partagé)
- [ ] `@astrojs/sitemap` configuré dans `astro.config.mjs`
- [ ] `site` renseigné dans `astro.config.mjs` (sinon canonical / sitemap cassés)
- [ ] Content Collections utilisées pour le contenu structuré
- [ ] `<Image>` ou `<Picture>` d'Astro pour l'optimisation
- [ ] `client:` directives utilisées avec parcimonie (par défaut zéro JS)
- [ ] `output: 'static'` sauf si besoin de SSR

### Pièges Astro
- 🚨 `client:load` sur des composants non-critiques → JS inutile
- 🚨 Oublier `site` dans config → canonical sera relative
- 🚨 Slugs auto-générés non-SEO-friendly depuis les collections

## SvelteKit

- [ ] `<svelte:head>` dans chaque `+page.svelte` pour les métas
- [ ] `+page.ts` `load` retourne les données SEO
- [ ] `prerender = true` dans `+layout.ts` pour les pages statiques
- [ ] `adapter-static` ou `adapter-vercel`/`-netlify` selon stratégie
- [ ] `app.html` minimal, métas globales dans `+layout.svelte`
- [ ] `hooks.server.ts` pour les redirects et headers

### Pièges SvelteKit
- 🚨 `ssr = false` global → désastre SEO
- 🚨 Oublier `prerender` sur pages statiques
- 🚨 Robots.txt et sitemap.xml en routes (`+server.ts`) non-prerender

## Remix

- [ ] Export `meta` dans chaque route
- [ ] Export `links` pour preload des assets critiques
- [ ] `loader` SSR pour données critiques
- [ ] `headers` export pour cache control
- [ ] Pas de logique SEO uniquement en `useEffect` (CSR-only)

### Pièges Remix
- 🚨 `defer` et `Await` sur du contenu SEO critique → contenu pas dans HTML initial
- 🚨 Oublier `meta` sur une route enfant → hérité du parent, parfois pas voulu

## Laravel (Blade)

- [ ] `@yield('title')`, `@yield('meta')` dans layout principal
- [ ] Chaque vue `@section('title', '...')`
- [ ] Routes nommées et URLs propres dans `routes/web.php`
- [ ] Sitemap généré (`spatie/laravel-sitemap` recommandé)
- [ ] Robots.txt dans `public/`
- [ ] Cache de page (`Cache-Control`) configuré

### Pièges Laravel
- 🚨 Routes en debug accessibles en prod (`/_ignition/`, `/telescope`)
- 🚨 `APP_DEBUG=true` en prod → fuite d'info et soft 404 ratés
- 🚨 Pas de canonical en cas de paramètres GET

## Symfony (Twig)

- [ ] `{% block title %}` et `{% block meta %}` dans `base.html.twig`
- [ ] `presta/sitemap-bundle` ou équivalent
- [ ] Routes annotées proprement (`#[Route]`)
- [ ] Cache HTTP via `HttpCache` ou Varnish/Symfony reverse proxy

## WordPress

- [ ] Un seul plugin SEO actif (Yoast OU Rank Math OU SEOPress — pas plusieurs)
- [ ] Thème ne duplique pas les balises gérées par le plugin SEO
- [ ] Permalinks en mode « Nom de l'article » (`/sample-post/`), pas `?p=123`
- [ ] Catégories & tags → stratégie : indexer ou non, pas les deux par défaut
- [ ] WP_DEBUG = false en prod
- [ ] `wp-cron` désactivé et remplacé par cron système si site à fort trafic
- [ ] Plugins de cache + image optim (WP Rocket / W3TC + Imagify / ShortPixel)
- [ ] Pas de plugins SEO obsolètes qui ajoutent leurs propres balises

### Pièges WordPress
- 🚨 Thème qui inclut son propre `<title>` + plugin SEO → double balise
- 🚨 « Décourager les moteurs de recherche d'indexer ce site » coché en prod (Réglages → Lecture)
- 🚨 Sitemap natif WP (`/wp-sitemap.xml`) **et** sitemap plugin → conflit
- 🚨 Permalinks `/?p=` → URLs non-SEO
- 🚨 Pages auteur, archives date, tags vides indexés par défaut

## Sites statiques (HTML brut)

- [ ] Build process qui injecte les métas par page (pas de copier-coller)
- [ ] Sitemap généré automatiquement (script ou outil)
- [ ] Robots.txt à la racine
- [ ] Pas de chemins en `file://` ou `localhost` oubliés
- [ ] Assets versionnés (hash dans le nom) pour le cache long

## React / Vue SPA (sans framework SSR)

- [ ] **Avertir explicitement** : SPA pur = handicap SEO important
- [ ] Suggérer migration vers framework SSR (Next, Nuxt, Remix, SvelteKit)
- [ ] OU pre-rendering au build (`react-snap`, `prerender-spa-plugin`, `vite-plugin-ssr`)
- [ ] OU dynamic rendering (Rendertron, Prerender.io) pour les bots
- [ ] React Helmet / Vue Meta installés et utilisés
- [ ] `<noscript>` fallback informatif
- [ ] Router HTML5 (`history` mode), pas hash router (`/#/page`)
- [ ] Serveur web configuré pour fallback `index.html` sur 404 client-side
