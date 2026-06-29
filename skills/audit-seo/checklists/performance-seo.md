# Checklist — Performance SEO & Core Web Vitals

Référencé par `SKILL.md` § 4.5.

> Les Core Web Vitals (LCP, INP, CLS) sont des facteurs de ranking confirmés.
> Sans crawl live + Lighthouse / PSI, l'audit reste **statique** : on identifie les anti-patterns dans le code, pas les valeurs mesurées.

## Largest Contentful Paint (LCP, cible < 2,5 s)

- [ ] L'image LCP probable (hero, premier visuel) est-elle :
  - [ ] servie en format moderne (AVIF, WebP) avec fallback
  - [ ] dimensionnée (`width`/`height`) pour éviter le layout shift
  - [ ] **non lazy-loadée** (`loading="eager"` pour le LCP, ou pas d'attribut)
  - [ ] préchargée via `<link rel="preload" as="image" fetchpriority="high">` quand pertinent
- [ ] La police principale est :
  - [ ] préchargée (`<link rel="preload" as="font" crossorigin>`)
  - [ ] `font-display: swap` (ou `optional`) pour éviter FOIT
  - [ ] self-hosted ou via CDN performant
- [ ] Pas de chaîne de requêtes critiques bloquante en `<head>` (CSS externes empilés, JS bloquant)
- [ ] SSR/SSG actif sur les pages publiques (pas de page critique en CSR pur)

## Interaction to Next Paint (INP, cible < 200 ms)

- [ ] JavaScript hors-route ne s'exécute pas (code-splitting par route)
- [ ] Pas de gros `useEffect` synchrones au mount
- [ ] Bundle initial raisonnable (< 200 KB gzip est un bon objectif)
- [ ] Hydration partielle / islands (Astro, Qwik) quand pertinent
- [ ] Pas de listeners globaux coûteux sur scroll/resize sans throttle/debounce
- [ ] Web workers pour les calculs lourds

## Cumulative Layout Shift (CLS, cible < 0,1)

- [ ] **Toutes** les images ont `width` et `height` (ou `aspect-ratio` en CSS)
- [ ] Vidéos et iframes ont des dimensions
- [ ] Pas de contenu injecté en haut de page après chargement (ads, banner cookie pushant le contenu)
- [ ] Fonts avec `size-adjust` ou `font-display: optional` pour limiter le reflow
- [ ] Skeleton / placeholders dimensionnés pour le contenu async

## Images

- [ ] Formats modernes (AVIF > WebP > JPEG) avec fallback
- [ ] Responsive : `srcset` et `sizes` corrects
- [ ] Composants framework utilisés : `next/image`, `<NuxtImg>`, `<Image>` Astro, etc.
- [ ] `loading="lazy"` sur tout sauf above-the-fold
- [ ] `decoding="async"` pour décodage non-bloquant
- [ ] Pas d'images de plusieurs MB en production
- [ ] CDN d'images configuré (Cloudinary, Imgix, Vercel, Cloudflare Images…)

## Lazy-loading

- [ ] Appliqué correctement (pas sur le LCP)
- [ ] iframes lazy-loadées (`loading="lazy"`) pour YouTube/Maps embeds
- [ ] Composants lourds chargés à la demande (`dynamic()`, `<Suspense>`, `defineAsyncComponent`)

## Fonts

- [ ] ≤ 2–3 familles de fontes
- [ ] ≤ 4–6 fichiers de fonte chargés
- [ ] `font-display: swap`
- [ ] Subset (latin, latin-ext) si site occidental, pour réduire la taille
- [ ] Preload du fichier critique uniquement

## JavaScript

- [ ] Scripts tiers (analytics, tag manager, chat, A/B) chargés en `defer` ou `async`
- [ ] GTM/GA chargés après interaction si possible (consent-deferred)
- [ ] Pas de bundles dupliqués (audit `npm ls`, `webpack-bundle-analyzer`)
- [ ] Tree-shaking effectif (imports nommés, pas d'import de bibliothèque entière)
- [ ] Polyfills servis uniquement aux navigateurs qui en ont besoin (`module`/`nomodule` ou ESM)

## CSS

- [ ] CSS critique inliné quand framework le permet (Astro, Nuxt avec `inlineStyles`)
- [ ] Pas de feuilles CSS énormes non utilisées (Tailwind avec purge, PurgeCSS)
- [ ] `<link rel="preload" as="style">` pour le CSS critique externe
- [ ] Pas de `@import` en chaîne dans le CSS principal

## Rendu côté client (CSR)

- [ ] Pages indexables critiques **ne sont pas** en CSR pur
- [ ] Si SPA → SSR via Next/Nuxt/SvelteKit/Remix activé
- [ ] Si Astro/site statique → contenu présent au build, pas hydraté
- [ ] Si CSR inévitable → pré-rendu (`prerender`, `react-snap`) pour les pages clés

## SSR / SSG

- [ ] Choix conscient page par page : `static`, `dynamic`, `revalidate`, `ISR`
- [ ] Pages produits / articles → SSG ou ISR
- [ ] Pages très dynamiques (compte, panier) → SSR ou CSR, mais `noindex`
- [ ] Pas de fetch bloquant inutile côté serveur qui ralentit le TTFB

## Cache & headers (si crawl disponible)

- [ ] HTML : `Cache-Control` adapté (ex. `s-maxage=60, stale-while-revalidate=300` pour CDN)
- [ ] Assets statiques avec hash : `Cache-Control: public, max-age=31536000, immutable`
- [ ] Gzip / Brotli activé
- [ ] HTTP/2 ou HTTP/3 activé
- [ ] `Vary` cohérent

## Pièges fréquents

- 🚨 `next/image` mal configuré (`unoptimized: true` en prod)
- 🚨 LCP en lazy → catastrophique
- 🚨 Google Fonts en `<link>` synchrone → bloque le rendu
- 🚨 Une seule bundle énorme sans code-splitting (vieux Webpack mal configuré)
- 🚨 Tag manager qui charge 30+ scripts tiers
- 🚨 React SPA sans SSR servi en prod avec un seul `<div id="root">` vide
