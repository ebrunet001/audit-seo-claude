# Référence — Seuils numériques SEO

Référencé par toutes les checklists. Sources : Google Search Central, web.dev, retours d'expérience industrie. Les valeurs sont **indicatives**, pas absolues.

## Métadonnées

| Élément | Min | Recommandé | Max | Notes |
|---|---|---|---|---|
| `<title>` | 30 | 50–60 | 60–70 | Tronqué en SERP au-delà de ~580 px (≈ 60 caractères latins) |
| Meta description | 70 | 120–160 | 160 | Mobile : ~120 ; desktop : jusqu'à 160 |
| URL slug | — | 3–5 mots | — | Privilégier la concision |
| H1 | 20 | 40–70 | 70 | Lisible humainement |
| Alt text | 5 | 10–125 caractères | 125 | Descriptif, pas du keyword stuffing |

## Open Graph

| Élément | Recommandation |
|---|---|
| `og:image` minimum | 600 × 315 px |
| `og:image` recommandé | **1200 × 630 px**, ratio 1.91:1 |
| `og:image` max | < 8 MB |
| `og:image` formats | JPG, PNG, WebP (PNG si transparence) |

## Twitter Cards

| Type | Image |
|---|---|
| `summary` | 144 × 144 px min |
| `summary_large_image` | **1200 × 628 px**, ratio 1.91:1, max 5 MB |

## Core Web Vitals (cibles « Good » de Google)

| Métrique | Good | Needs improvement | Poor |
|---|---|---|---|
| **LCP** (Largest Contentful Paint) | ≤ 2,5 s | ≤ 4 s | > 4 s |
| **INP** (Interaction to Next Paint) | ≤ 200 ms | ≤ 500 ms | > 500 ms |
| **CLS** (Cumulative Layout Shift) | ≤ 0,1 | ≤ 0,25 | > 0,25 |
| **FCP** (First Contentful Paint) | ≤ 1,8 s | ≤ 3 s | > 3 s |
| **TTFB** (Time to First Byte) | ≤ 800 ms | ≤ 1,8 s | > 1,8 s |

> Mesuré au 75e percentile sur trafic réel (CrUX), pas en lab seul.

## Performance — autres seuils utiles

| Métrique | Cible |
|---|---|
| JS initial (gzip) | < 200 KB |
| HTML initial (gzip) | < 30 KB |
| Image LCP | < 200 KB |
| Total page weight | < 1,5 MB |
| Nombre de requêtes | < 50 |
| Domaines tiers | < 10 |

## Sitemap

| Limite | Valeur |
|---|---|
| URLs par sitemap | ≤ 50 000 |
| Taille fichier sitemap | ≤ 50 MB non compressé |
| Sitemap index | jusqu'à 50 000 sitemaps |

## URLs

| Élément | Recommandation |
|---|---|
| Longueur URL | < 75 caractères idéal, < 2048 octets max |
| Profondeur de clic (depuis home) | ≤ 3 pour pages importantes |
| Profondeur de dossier | ≤ 4 niveaux |

## Contenu

| Type de page | Mots indicatifs |
|---|---|
| Page produit (e-commerce) | 200–500 mots de contenu unique |
| Article de blog | 800–2000 mots selon sujet |
| Page pilier / guide | 2000+ mots |
| Page catégorie | 150–300 mots d'intro |
| Landing page | 300–800 mots |

> Ce sont des indicateurs, pas une vérité. La pertinence prime sur le volume.

## Liens

| Élément | Recommandation |
|---|---|
| Liens internes par page | 30–150 selon le type de page |
| Liens externes par page | < 100 |
| Liens sortants dofollow vers même domaine externe | < 5 |

## Images

| Élément | Recommandation |
|---|---|
| Largeur max servie | 2× la largeur d'affichage (pour Retina) |
| Format moderne | AVIF > WebP > JPEG > PNG (selon contenu) |
| Compression JPEG | qualité 75–85 |
| Taille fichier raisonnable | < 200 KB pour photos, < 50 KB pour illus |

## Headers HTTP utiles

| Header | Valeur recommandée |
|---|---|
| `Cache-Control` (HTML dynamique) | `public, s-maxage=60, stale-while-revalidate=300` |
| `Cache-Control` (assets hashés) | `public, max-age=31536000, immutable` |
| `Content-Security-Policy` | (cf. sécurité, n'impacte pas directement le SEO) |
| `Strict-Transport-Security` | `max-age=63072000; includeSubDomains; preload` |
| `X-Robots-Tag` | utilisé en alternative à meta robots quand HTML non modifiable |

## Score /100 — barème indicatif du skill

Le skill calcule un score indicatif :

```
score = max(0, 100 - 15 × n_critique - 8 × n_élevé - 3 × n_moyen - 1 × n_faible)
```

| Score | Interprétation |
|---|---|
| 90–100 | Excellent, polish uniquement |
| 75–89 | Bon, quelques optimisations à faire |
| 60–74 | Correct, plusieurs chantiers identifiés |
| 40–59 | Faible, refactor SEO nécessaire |
| 0–39 | Critique, problèmes structurels |

Ce score est **indicatif** et présenté comme tel dans le rapport.
