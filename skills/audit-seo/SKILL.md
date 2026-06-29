---
name: audit-seo
description: Performs a comprehensive SEO audit of a web project from its source code, configuration, and optionally a live URL. Use this skill when the user asks for an SEO audit, a technical SEO review, a metadata or Open Graph / Twitter Card analysis, a robots.txt or sitemap.xml verification, a canonical / hreflang / redirects review, a structured data check (JSON-LD, Schema.org), a Core Web Vitals or performance-SEO review, an internal linking and content audit, or any general SEO optimization of a web project. Triggers on requests like "audit SEO", "revue SEO", "analyse SEO", "/audit-seo", "vérifie mon SEO", "optimisation SEO", "SEO review", "check my SEO", "technical SEO". Supports Next.js, Nuxt, Astro, SvelteKit, Remix, Laravel, Symfony, WordPress, static sites, and React/Vue SPAs.
---

# audit-seo

Audit SEO complet d'un projet web à partir de son code source, de sa configuration, et — si une URL est fournie — d'un environnement local ou de production.

> Le skill **n'écrit jamais** dans le projet sans confirmation explicite. Il **n'exécute pas** de commandes destructives, ni de déploiement, ni de publication.

---

## 1. Quand activer ce skill

Active ce skill quand l'utilisateur demande, en français ou en anglais :

- un audit / une revue / une analyse SEO (technique, on-page, structurelle, de contenu)
- une vérification de `robots.txt`, `sitemap.xml`, canoniques, redirections, hreflang
- une analyse de métadonnées (`<title>`, meta description, Open Graph, Twitter Cards)
- une vérification des données structurées (JSON-LD, Schema.org, rich results)
- une revue de performance liée au SEO (Core Web Vitals, images, JS bloquant, SSR/SSG)
- un audit de contenu (duplication, cannibalisation, maillage interne, slugs)
- l'invocation explicite via `/audit-seo`

---

## 1bis. Boundaries — périmètre du skill

Ce skill audite des projets web **génériques** : Next.js, Nuxt, Astro, SvelteKit, Remix, Laravel, Symfony, WordPress, site statique, SPA React/Vue. Il couvre le SEO **technique, on-page, structurel et de contenu** de ces projets.

Ce qui est **hors périmètre** (à traiter avec un outil ou une expertise dédiée) :

- les pages de **marketplaces ou stores tiers** au gabarit imposé (sections auto-générées, onglets de pricing rendus par la plateforme, descriptions de produit normalisées) — l'optimisation y suit les conventions propres à la plateforme, pas les règles SEO web classiques ;
- la **stratégie de pricing / monétisation** d'un produit ou service ;
- l'**ingénierie de collecte de données** (crawlers, anti-bot, proxies) — sans rapport avec le SEO du projet audité.

Pour les concerns AI-search / AEO / GEO (LLM crawlers, `llms.txt`, citations en AI Overview), voir `checklists/ai-search-seo.md`.

---

## 2. Arguments acceptés (`$ARGUMENTS`)

Le skill accepte des arguments libres après l'invocation. Parse `$ARGUMENTS` pour extraire :

| Argument | Effet |
|---|---|
| première URL `http(s)://...` trouvée | cible de crawl (local ou prod) |
| `focus=technical\|metadata\|content\|structured-data\|performance\|all` | restreint les catégories auditées (défaut : `all`) |
| `output=markdown\|json` | format du rapport (défaut : `markdown`) |
| `depth=shallow\|standard\|deep` | profondeur de l'audit (défaut : `standard`) |

**Exemples d'invocation :**

```text
/audit-seo
/audit-seo http://localhost:3000
/audit-seo https://example.com
/audit-seo focus=technical
/audit-seo focus=metadata
/audit-seo focus=content
/audit-seo https://example.com focus=structured-data depth=deep
/audit-seo output=markdown
```

Si aucun argument n'est fourni, exécute l'audit complet sur le code source uniquement.

---

## 3. Workflow d'exécution

Suis cette séquence dans l'ordre. Ne saute pas d'étape silencieusement — si une étape est impossible, indique-le dans le rapport final dans la section **« Limites de l'audit »**.

### Étape 1 — Détection de la stack

Inspecte la racine du projet pour identifier le framework :

- `package.json` → cherche `next`, `nuxt`, `astro`, `@sveltejs/kit`, `@remix-run/*`, `vite`, `react`, `vue`
- `composer.json` → cherche `laravel/framework`, `symfony/symfony`
- `wp-config.php`, `wp-content/` → WordPress
- `astro.config.*`, `next.config.*`, `nuxt.config.*`, `svelte.config.*`, `remix.config.*`, `vite.config.*`
- Absence de framework → site statique HTML/CSS/JS

Consulte `references/framework-map.md` pour la correspondance fichiers ↔ framework et les pièges SEO spécifiques à chaque framework.

### Étape 2 — Inventaire des fichiers critiques

Liste, sans lire intégralement, les fichiers SEO-pertinents :

- **Config** : `next.config.*`, `nuxt.config.*`, `astro.config.*`, `vite.config.*`, `svelte.config.*`, `remix.config.*`
- **Robots & sitemap** : `public/robots.txt`, `static/robots.txt`, `robots.txt`, `public/sitemap.xml`, route `/sitemap.xml`, `app/robots.ts`, `app/sitemap.ts` (Next.js App Router)
- **Routes & pages** : `app/`, `pages/`, `src/pages/`, `src/routes/`, `routes/`
- **Layouts & head** : `app/layout.*`, `pages/_app.*`, `pages/_document.*`, `src/layouts/`, composants `Head`, `SEO`, `Meta`, `<svelte:head>`
- **Assets** : `public/`, `static/`, `assets/`
- **i18n** : `i18n/`, `locales/`, `next-intl`, `nuxt-i18n`, `@astrolicious/i18n`

### Étape 3 — Détection des routes

- Énumère les routes statiques et dynamiques du framework.
- Détecte les pages probablement indexables (publiques, sans `noindex`).
- Repère les routes orphelines (non liées depuis aucune autre page) ou les 404 connus.

### Étape 4 — Audit du code source

Exécute, en parallèle quand possible, les checklists des catégories demandées (`focus=...`) ou toutes si `focus=all`. Référence-toi à :

- `checklists/technical-seo.md`
- `checklists/metadata-seo.md`
- `checklists/structured-data.md`
- `checklists/performance-seo.md`
- `checklists/content-seo.md`
- `checklists/framework-specific.md`

Pour chaque finding, capture :
- la sévérité (`Critique`, `Élevée`, `Moyenne`, `Faible`)
- la catégorie
- le chemin du fichier et le numéro de ligne quand pertinent
- la preuve (extrait de code, valeur observée)
- la correction recommandée avec extrait de code suggéré

### Étape 5 — Crawl optionnel (URL fournie)

Si une URL est fournie dans `$ARGUMENTS` :

1. Vérifie d'abord que l'URL est `http://localhost:*` ou un domaine dont l'utilisateur est propriétaire. **En cas de doute, demande confirmation avant de crawler.**
2. Récupère `GET /` et inspecte :
   - status code et chaîne de redirections
   - `<head>` complet (title, meta, og, twitter, canonical, hreflang, JSON-LD)
   - balises `<h1>` à `<h6>`, attributs `alt`, liens internes
3. Récupère `/robots.txt` et `/sitemap.xml` (et sitemaps imbriqués).
4. Échantillonne 3 à 10 URLs depuis le sitemap pour vérifier la cohérence des templates.
5. Si l'utilisateur n'a fourni aucune URL, **ne crawle rien** — limite-toi à l'analyse statique.

> Si tu n'as pas accès au réseau, ou si tu ne peux pas exécuter de fetch, indique-le dans **« Limites de l'audit »** et continue uniquement avec l'analyse statique.

### Étape 6 — Priorisation

Trie les findings par impact SEO × effort :

- **Critique** : bloque l'indexation ou casse le crawl (`robots.txt` qui bloque tout, `noindex` global, sitemap invalide, redirect loop).
- **Élevée** : pénalise fortement le ranking (titles dupliqués, absence de canonical, H1 manquants, données structurées invalides).
- **Moyenne** : opportunité significative (Open Graph manquant, alt manquants, slugs faibles, maillage interne pauvre).
- **Faible** : polish (cohérence stylistique des titles, méta description sub-optimale).

### Étape 7 — Génération du rapport

Utilise `templates/report.md` comme structure. Le rapport est en **Markdown** par défaut. Si `output=json`, produis également un bloc JSON structuré final compatible avec un traitement automatisé.

### Étape 8 — Propositions de correction

Pour les findings priorisés, prépare des correctifs **sous forme de propositions** (extraits de code, diffs). **Ne modifie aucun fichier sans demander confirmation explicite à l'utilisateur**, fichier par fichier ou par lot clairement identifié.

---

## 4. Catégories auditées

### 4.1 SEO technique
Voir `checklists/technical-seo.md`. Couvre : robots.txt, sitemap.xml, canoniques, redirections, status codes, noindex/nofollow, structure d'URL, pagination, hreflang, 404, routing framework.

### 4.2 Métadonnées
Voir `checklists/metadata-seo.md`. Couvre : `<title>`, meta description, Open Graph, Twitter Cards, canonical, viewport, charset, robots meta, cohérence inter-templates.

### 4.3 Structure HTML
Voir `checklists/content-seo.md` (section « Structure HTML »). Couvre : H1 unique, hiérarchie Hn, liens internes, attributs `alt`, liens cassés, sémantique HTML, a11y SEO.

### 4.4 Données structurées
Voir `checklists/structured-data.md`. Couvre : JSON-LD, Schema.org (`Organization`, `WebSite`, `BreadcrumbList`, `Article`, `Product`, `FAQPage`), cohérence et validation.

### 4.5 Performance & Core Web Vitals
Voir `checklists/performance-seo.md`. Couvre : images non optimisées, lazy-loading, fonts, JS bloquant, CSS critique, bundle, rendu CSR excessif, choix SSR/SSG.

### 4.6 Contenu & maillage
Voir `checklists/content-seo.md`. Couvre : duplication, cannibalisation, intention de recherche, maillage interne, profondeur de clic, slugs, cohérence langue/pays.

### 4.7 Spécificités framework
Voir `checklists/framework-specific.md`. Couvre les pièges propres à chaque stack (App Router vs Pages Router Next.js, `useHead` Nuxt, content collections Astro, etc.).

### 4.8 AI search & AEO/GEO (2026)
Voir `checklists/ai-search-seo.md`. Couvre les concerns SEO de l'ère AI-search : directives AI-crawlers dans `robots.txt` (GPTBot, ClaudeBot, Google-Extended, PerplexityBot, Applebot-Extended, CCBot), standard émergent `llms.txt`/`llms-full.txt`, optimisation pour AI Overview / SGE / Perplexity citations, E-E-A-T pour AI search, anti-patterns spécifiques. Active cette catégorie systématiquement en 2026 — bloquer tous les `*Bot` AI sans réfléchir coupe l'opportunité d'apparaître dans les AI Overviews; ne pas avoir de stratégie explicite = défaut d'autorisation implicite, pas un choix.

---

## 5. Format de sortie du rapport

Suis exactement la structure de `templates/report.md`. Sections obligatoires :

1. **Résumé exécutif** — score /100, 3 à 5 problèmes prioritaires, impact estimé, urgence
2. **Points critiques** — détaillés un par un
3. **Audit technique**
4. **Métadonnées**
5. **Structure HTML**
6. **Données structurées**
7. **Performance SEO**
8. **Contenu et maillage interne**
9. **Recommandations priorisées** — tableau (Priorité, Action, Effort, Impact, Fichier/zone)
10. **Plan d'action** — immédiat / cette semaine / à planifier
11. **Limites de l'audit** — ce qui n'a pas pu être vérifié et pourquoi

Le score /100 est une **estimation indicative**, jamais présentée comme une mesure absolue. Documente brièvement la méthode de calcul (somme pondérée des pénalités par sévérité).

---

## 6. Contraintes de sécurité

À respecter **sans exception** :

- ❌ Ne jamais lancer de commande destructive (`rm`, `git push --force`, `npm publish`, `wp-cli` destructeur, etc.).
- ❌ Ne jamais publier, déployer, supprimer ou modifier de fichier sans confirmation explicite **dans le chat**.
- ❌ Ne jamais exécuter de migration, de build de production, de commande de déploiement.
- ❌ Ne jamais crawler un domaine tiers sans confirmation explicite de l'utilisateur (anti-abus).
- ⚠️ Demander confirmation avant toute écriture, fichier par fichier ou par lot identifié.
- ⚠️ Signaler clairement les limites de l'audit (pas d'accès réseau, pas de build, pas de métriques réelles, etc.) dans la section dédiée du rapport.
- ✅ Lectures seules autorisées : `cat`, `ls`, `grep`, `find`, lecture de configs, parsing de HTML.
- ✅ Fetch en lecture seule autorisé uniquement sur l'URL fournie par l'utilisateur.

---

## 7. Fichiers de support

Chargés automatiquement en contexte selon les besoins :

- `templates/report.md` — squelette exact du rapport final
- `checklists/technical-seo.md` — checklist SEO technique
- `checklists/metadata-seo.md` — checklist métadonnées
- `checklists/structured-data.md` — checklist données structurées
- `checklists/performance-seo.md` — checklist performance / Core Web Vitals
- `checklists/content-seo.md` — checklist contenu, structure HTML, maillage
- `checklists/framework-specific.md` — pièges par framework
- `checklists/ai-search-seo.md` — AI-crawlers, `llms.txt`, AI Overview / SGE, E-E-A-T pour AI search (2026)
- `references/framework-map.md` — correspondance fichiers ↔ framework
- `references/seo-thresholds.md` — seuils numériques (longueur title, description, etc.)

Consulte ces fichiers à la demande pendant l'audit ; ne les recopie pas intégralement dans le rapport.
