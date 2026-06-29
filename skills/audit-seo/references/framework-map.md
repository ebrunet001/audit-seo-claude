# Référence — Correspondance fichiers ↔ framework

Référencé par `SKILL.md` § 3 étape 1.

Carte rapide des fichiers à inspecter selon la stack détectée.

## Détection rapide

| Signal détecté | Stack |
|---|---|
| `package.json` contient `"next"` | Next.js (vérifier App vs Pages Router) |
| `package.json` contient `"nuxt"` | Nuxt 3 (ou Nuxt 2 si version < 3) |
| `package.json` contient `"astro"` | Astro |
| `package.json` contient `"@sveltejs/kit"` | SvelteKit |
| `package.json` contient `"@remix-run/*"` | Remix |
| `package.json` contient `"vite"` + `"react"` sans framework | React SPA (Vite) |
| `package.json` contient `"vite"` + `"vue"` sans framework | Vue SPA (Vite) |
| `package.json` contient `"react"` + `"react-scripts"` | Create React App (legacy SPA) |
| `package.json` contient `"@angular/core"` | Angular |
| `composer.json` contient `"laravel/framework"` | Laravel |
| `composer.json` contient `"symfony/symfony"` ou `"symfony/framework-bundle"` | Symfony |
| `wp-config.php` à la racine ou présence de `wp-content/` | WordPress |
| Que des fichiers `.html` / `.css` / `.js` à la racine | Site statique |
| `gatsby-config.js` | Gatsby (legacy) |
| `eleventy.config.js` ou `.eleventy.js` | 11ty |

## Fichiers à inspecter par framework

### Next.js
- `next.config.js` / `next.config.mjs` / `next.config.ts`
- `app/layout.tsx`, `app/page.tsx`, `app/**/page.tsx` (App Router)
- `pages/_app.tsx`, `pages/_document.tsx`, `pages/index.tsx`, `pages/**/*.tsx` (Pages Router)
- `app/robots.ts`, `app/sitemap.ts`
- `app/icon.tsx`, `app/opengraph-image.tsx`
- `middleware.ts`
- `public/robots.txt`, `public/sitemap.xml`
- `next-seo.config.js` si `next-seo` installé

### Nuxt
- `nuxt.config.ts`
- `app.vue`
- `pages/**/*.vue`
- `layouts/**/*.vue`
- `composables/useSEO.*` ou équivalent
- `server/routes/sitemap.xml.ts`, `server/routes/robots.txt.ts`
- `public/robots.txt`, `public/sitemap.xml`

### Astro
- `astro.config.mjs` / `.ts`
- `src/pages/**/*.astro`, `**/*.md`, `**/*.mdx`
- `src/layouts/**/*.astro`
- `src/components/SEO.astro` (ou similaire)
- `src/content/config.ts` (Content Collections)
- `public/robots.txt`
- Sitemap généré par `@astrojs/sitemap`

### SvelteKit
- `svelte.config.js`
- `vite.config.js`
- `src/app.html`
- `src/routes/**/+page.svelte`, `+page.ts`, `+page.server.ts`
- `src/routes/**/+layout.svelte`, `+layout.ts`
- `src/routes/robots.txt/+server.ts`
- `src/routes/sitemap.xml/+server.ts`
- `src/hooks.server.ts`

### Remix
- `remix.config.js`
- `app/root.tsx`
- `app/routes/**/*.tsx`
- `app/entry.server.tsx`
- `public/robots.txt`

### Laravel
- `composer.json`
- `routes/web.php`
- `resources/views/**/*.blade.php`
- `resources/views/layouts/app.blade.php`
- `app/Http/Controllers/**`
- `config/app.php` (URL, locale)
- `public/robots.txt`, `public/sitemap.xml`

### Symfony
- `composer.json`
- `config/routes.yaml`, `config/routes/`
- `templates/base.html.twig`
- `templates/**/*.html.twig`
- `src/Controller/**`
- `public/robots.txt`

### WordPress
- `wp-config.php`
- `wp-content/themes/{theme-actif}/functions.php`
- `wp-content/themes/{theme-actif}/header.php`
- `wp-content/themes/{theme-actif}/footer.php`
- `wp-content/themes/{theme-actif}/single.php`, `page.php`, `index.php`
- Plugins SEO actifs (vérifier dans `wp-content/plugins/` : yoast-seo, seo-by-rank-math, wp-seopress)
- `.htaccess` à la racine
- `robots.txt` (souvent virtuel, généré par plugin)

### Sites statiques
- Tous les `*.html` à la racine et sous-dossiers
- `css/`, `js/`, `assets/`
- `robots.txt`, `sitemap.xml` ou `sitemap.txt`
- Outil de build éventuel (Gulp, scripts npm, Webpack)

### SPA React/Vue (sans framework)
- `index.html` racine (souvent dans `public/` ou `src/`)
- `src/main.tsx` / `src/main.js`
- `src/App.tsx` / `src/App.vue`
- Router configuration (`react-router-dom`, `vue-router`)
- `vite.config.*` ou `webpack.config.*`

## Commandes utiles (lecture seule)

```bash
# Détection rapide de la stack
cat package.json 2>/dev/null | grep -E '"(next|nuxt|astro|@sveltejs/kit|@remix-run|react|vue|@angular/core|vite)"'
cat composer.json 2>/dev/null | grep -E '"(laravel|symfony)"'
ls wp-config.php wp-content 2>/dev/null

# Recherche de fichiers SEO clés
find . -maxdepth 4 \( -name 'robots.txt' -o -name 'sitemap.xml' -o -name 'sitemap.ts' -o -name 'robots.ts' \) -not -path '*/node_modules/*' -not -path '*/vendor/*' 2>/dev/null

# Recherche des composants SEO/Head
grep -rE '(useHead|useSeoMeta|<Head>|<svelte:head>|export const meta|generateMetadata|metadata =)' --include='*.tsx' --include='*.ts' --include='*.vue' --include='*.svelte' --include='*.astro' . 2>/dev/null | head -30

# Énumération des routes (Next.js App Router)
find app -name 'page.tsx' -o -name 'page.jsx' -o -name 'page.ts' -o -name 'page.js' 2>/dev/null

# Énumération des routes (Nuxt / Astro / SvelteKit)
find pages src/pages src/routes -name '*.vue' -o -name '*.astro' -o -name '+page.svelte' 2>/dev/null
```
