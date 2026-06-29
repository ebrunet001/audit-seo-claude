<!--
Squelette de rapport pour le skill audit-seo.
Remplace toutes les {{placeholders}} et les sections "À compléter".
Conserve l'ordre des sections.
-->

# Audit SEO — {{nom_du_projet_ou_url}}

> Date : {{date}}
> Stack détectée : {{framework}} {{version}}
> Cible : {{code_source_ou_url}}
> Profondeur : {{shallow|standard|deep}}

---

## Résumé exécutif

- **Score global estimé** : **{{score}}/100**
  *Méthode : score initial 100, pénalités cumulées par finding selon sévérité (Critique −15, Élevée −8, Moyenne −3, Faible −1), plancher à 0.*
- **Niveau d'urgence** : {{Critique | Élevé | Modéré | Faible}}
- **Impact SEO estimé** : {{description courte de l'impact sur l'indexation, le ranking et le CTR}}

### Top 3 à 5 problèmes prioritaires
1. {{problème — fichier/url — impact}}
2. {{...}}
3. {{...}}

---

## Points critiques

> Liste détaillée des findings de sévérité **Critique** et **Élevée**.
> Pour les sévérités Moyenne et Faible, voir les sections par catégorie ci-dessous.

### 🟥 Finding #1 — {{titre}}
- **Sévérité** : Critique
- **Catégorie** : {{Technique | Métadonnées | Structure HTML | Données structurées | Performance | Contenu}}
- **Fichier ou URL** : `{{chemin/fichier.ext:ligne}}` ou `{{https://...}}`
- **Preuve** :
  ```{{lang}}
  {{extrait observé}}
  ```
- **Pourquoi c'est un problème** : {{explication concise, 1-3 phrases}}
- **Correction recommandée** : {{description}}
- **Exemple de correction** :
  ```{{lang}}
  {{extrait corrigé}}
  ```

### 🟧 Finding #2 — {{titre}}
*(même structure)*

---

## Audit technique

- [ ] `robots.txt` — {{statut + remarques}}
- [ ] `sitemap.xml` — {{statut + remarques}}
- [ ] Canonical URLs — {{statut}}
- [ ] Redirections — {{statut}}
- [ ] Status codes — {{statut}}
- [ ] `noindex` / `nofollow` — {{statut}}
- [ ] Structure des URLs — {{statut}}
- [ ] Pagination — {{statut}}
- [ ] hreflang — {{statut ou N/A}}
- [ ] 404 et routes orphelines — {{statut}}
- [ ] Routing framework — {{statut}}

**Findings additionnels** :
{{liste des findings Moyenne/Faible}}

---

## Métadonnées

- [ ] `<title>` — longueur, unicité, cohérence par template
- [ ] Meta description — longueur, unicité, présence
- [ ] Open Graph (`og:title`, `og:description`, `og:image`, `og:url`, `og:type`)
- [ ] Twitter Cards (`twitter:card`, `twitter:title`, `twitter:image`)
- [ ] Canonical
- [ ] Viewport
- [ ] Charset
- [ ] Robots meta

**Findings** :
{{liste}}

---

## Structure HTML

- [ ] Un seul `<h1>` par page (quand pertinent)
- [ ] Hiérarchie Hn logique (pas de saut H1 → H3)
- [ ] Liens internes — quantité et qualité des ancres
- [ ] Attributs `alt` sur toutes les images de contenu
- [ ] Liens cassés ou suspects
- [ ] Sémantique HTML (`<main>`, `<article>`, `<nav>`, `<header>`, `<footer>`)
- [ ] Accessibilité ayant impact SEO

**Findings** :
{{liste}}

---

## Données structurées

- [ ] JSON-LD présent
- [ ] `Organization` ou `WebSite` global
- [ ] `BreadcrumbList` sur pages profondes
- [ ] Types contextuels (`Article`, `Product`, `FAQPage`, `Recipe`, `LocalBusiness`…)
- [ ] Validation syntaxique
- [ ] Cohérence entre JSON-LD et contenu visible

**Findings** :
{{liste}}

---

## Performance SEO

- [ ] Images optimisées (format moderne, dimensions, `loading`, `decoding`)
- [ ] Lazy-loading correctement appliqué
- [ ] Fonts (`font-display`, preload, self-hosted vs CDN)
- [ ] JavaScript bloquant (`defer`, `async`, taille des bundles)
- [ ] CSS critique
- [ ] Bundle size estimé
- [ ] Rendu CSR vs SSR/SSG
- [ ] Cache et headers (si crawl effectué)

**Findings** :
{{liste}}

---

## Contenu et maillage interne

- [ ] Titres et descriptions dupliqués
- [ ] Pages pauvres en contenu (thin content)
- [ ] Cannibalisation potentielle (pages se chevauchant sur même intention)
- [ ] Intention de recherche alignée avec contenu
- [ ] Maillage interne — profondeur de clic, ancres descriptives
- [ ] Slugs (longueur, lisibilité, mots-clés)
- [ ] Cohérence langue/pays (lang HTML, hreflang, contenu localisé)

**Findings** :
{{liste}}

---

## Recommandations priorisées

| Priorité | Action | Effort | Impact | Fichier / zone |
|---|---|---|---|---|
| P0 | {{action critique}} | S | XL | `{{chemin}}` |
| P0 | {{...}} | M | XL | `{{...}}` |
| P1 | {{...}} | S | L | `{{...}}` |
| P2 | {{...}} | M | M | `{{...}}` |
| P3 | {{...}} | L | S | `{{...}}` |

> Effort : S = < 1h, M = 1–4h, L = 0,5–2j, XL = > 2j.
> Impact : S/M/L/XL — impact SEO estimé.

---

## Plan d'action

### 🚨 Actions immédiates (à faire aujourd'hui)
- {{action}}
- {{action}}

### 📅 Actions à faire cette semaine
- {{action}}
- {{action}}

### 🗓️ Actions à planifier
- {{action}}
- {{action}}

---

## Limites de l'audit

> Ce qui n'a **pas** pu être vérifié et pourquoi. À remplir honnêtement.

- {{ex : pas d'accès au site en production → pas de vérification de status codes réels, ni de Core Web Vitals mesurés}}
- {{ex : pas de build exécuté → pas d'analyse de bundle réel}}
- {{ex : pas d'accès à Google Search Console → pas de données de couverture d'index}}
- {{ex : crawl limité à 10 URLs échantillonnées}}

---

*Rapport généré par le skill `audit-seo`. Aucun fichier du projet n'a été modifié.*
