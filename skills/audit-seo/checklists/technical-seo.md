# Checklist — SEO technique

Référencé par `SKILL.md` § 4.1 et § 3 étape 4.

## robots.txt

- [ ] Présent à la racine du domaine (`/robots.txt`, servi en `text/plain`, status 200)
- [ ] Ne bloque pas accidentellement tout le site (`Disallow: /` global sur env de prod)
- [ ] Bloque les zones non-publiques (admin, panier, recherche interne, API)
- [ ] Référence le ou les `Sitemap:` absolus en bas du fichier
- [ ] Pas de directives obsolètes (`Noindex:`, `Crawl-delay` excessif)
- [ ] Distinction entre robots.txt de staging et de prod (souvent oubliée)

**À chercher dans le code** :
- `public/robots.txt`, `static/robots.txt`, `app/robots.ts` (Next.js), `app/robots.txt/route.ts`
- Configuration `nuxt-simple-sitemap`, `@astrojs/sitemap`, plugins WordPress (Yoast, Rank Math)

## sitemap.xml

- [ ] Présent et accessible (status 200, `application/xml`)
- [ ] XML valide (déclaration, namespace `http://www.sitemaps.org/schemas/sitemap/0.9`)
- [ ] Liste **uniquement** des URLs canoniques, indexables, 200
- [ ] `<lastmod>` cohérent (pas tous identiques, pas tous = aujourd'hui)
- [ ] < 50 000 URLs et < 50 MB par fichier, sinon sitemap index
- [ ] Pas d'URLs avec `noindex`, paramètres de tracking, ou redirections
- [ ] Pour multi-langue : un sitemap par locale OU `xhtml:link` hreflang dans chaque entrée
- [ ] Sitemap déclaré dans `robots.txt` et soumis à Google Search Console

**À chercher** :
- `public/sitemap.xml`, génération via `next-sitemap`, `@astrojs/sitemap`, `nuxt/sitemap`, `sveltekit-adapter-static` + script, Symfony bundle, plugin WP

## Canonical URLs

- [ ] Chaque page indexable a une `<link rel="canonical" href="...">` absolue
- [ ] Canonical pointe vers une URL **200**, pas une redirection ou une 404
- [ ] Canonical cohérent avec `og:url` et l'URL du sitemap
- [ ] Pas de canonical en double sur une page
- [ ] Pour pagination : `?page=2` se canonicalise sur lui-même (pas sur page 1, sauf stratégie consciente)
- [ ] Pas de canonical générique pointant tout vers la home

## Redirections

- [ ] Pas de chaîne de redirections (> 1 hop = pénalisant)
- [ ] HTTP → HTTPS unique et 301
- [ ] `www` → apex (ou inverse) unique et 301
- [ ] Trailing slash : règle unique et cohérente
- [ ] Pas de boucles de redirection
- [ ] Redirections 301 (permanentes) pour les changements définitifs, 302 uniquement pour le temporaire
- [ ] Anciennes URLs après refonte → redirection vers nouvel équivalent **précis** (pas la home)

## Status codes

- [ ] Pages publiques → 200
- [ ] Pages supprimées → 404 ou 410 (410 si volontairement supprimées)
- [ ] Pages en maintenance → 503 + `Retry-After`
- [ ] Pas de 200 sur des pages d'erreur (« soft 404 »)

## noindex / nofollow

- [ ] `<meta name="robots" content="noindex">` **absent** des pages publiques
- [ ] Présent volontairement sur : pages de recherche interne, paniers, login, mentions duplicate
- [ ] Cohérence entre `meta robots`, `X-Robots-Tag` header, et `robots.txt`
- [ ] Sur environnement de staging : `noindex` global recommandé

## Structure des URLs

- [ ] URLs lisibles, en minuscules, mots séparés par `-`
- [ ] Pas de paramètres inutiles (`?utm_source` sans canonical, `?ref=`, IDs de session)
- [ ] Profondeur ≤ 3 niveaux pour les pages importantes
- [ ] Slugs descriptifs, pas d'IDs numériques seuls
- [ ] Cohérence singulier/pluriel sur l'ensemble du site
- [ ] Pas de duplication par variation (avec/sans slash, http/https, www/non-www)

## Pagination

- [ ] Chaque page paginée a sa propre URL (`?page=2` ou `/page/2`)
- [ ] Canonical de chaque page paginée pointe vers elle-même
- [ ] Liens « page suivante » crawlables (`<a href>`, pas onClick JS)
- [ ] `rel="next"` / `rel="prev"` ne sont plus utilisés par Google mais ne nuisent pas
- [ ] Évite la pagination en infinite scroll sans fallback paginé crawlable

## hreflang

*(uniquement si site multilingue ou multi-régional)*

- [ ] Présent sur chaque variante linguistique
- [ ] Réciprocité complète (A → B implique B → A)
- [ ] `x-default` pour la version par défaut
- [ ] Codes valides (`fr`, `fr-FR`, `en-US`, etc.) — pas `fr-fr`
- [ ] Cohérence avec `<html lang="...">` et le contenu réel
- [ ] Implémenté soit dans `<head>`, soit dans le sitemap, soit en HTTP headers — un seul endroit

## 404 & routes orphelines

- [ ] Page 404 personnalisée, utile (liens vers home, recherche, sections)
- [ ] Page 404 renvoie bien un code **404** (pas 200)
- [ ] Liens internes ne pointent pas vers des 404
- [ ] Routes existantes mais non liées (orphelines) → soit liées, soit `noindex`, soit supprimées

## Erreurs de routing framework

- [ ] Pas de routes en double (ex. Next.js `app/` et `pages/` qui se chevauchent)
- [ ] Routes dynamiques `[slug]` ont un fallback / 404 propre pour valeurs inconnues
- [ ] `generateStaticParams` / `getStaticPaths` couvrent les URLs critiques
- [ ] Pas de routes catch-all qui mangent silencieusement des 404 légitimes
- [ ] Côté SPA : routes accessibles directement par URL (pas seulement après navigation interne)
