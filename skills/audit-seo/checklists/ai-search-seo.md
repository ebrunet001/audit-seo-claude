# Checklist — AI search & AEO/GEO (2026)

Référencé par `SKILL.md` § 4.8. Couvre les concerns SEO spécifiques à l'ère AI-search : crawlers LLM, AI Overview (Google SGE), citations Perplexity/ChatGPT browsing, et standards émergents (`llms.txt`).

> **Note de scope :** ces concerns sont génériques web. Les pages de marketplaces ou stores tiers au gabarit imposé suivent les conventions propres à leur plateforme et sortent du périmètre de ce skill (voir § 1bis du `SKILL.md`).

## AI-crawler directives dans `robots.txt`

- [ ] **Décision documentée** : autoriser ou bloquer les LLM-trainers ? L'absence de directive = autorisation implicite, ce qui est souvent un défaut par défaut, pas un choix conscient.
- [ ] **`User-agent: GPTBot`** — crawler OpenAI pour entraînement de modèles
- [ ] **`User-agent: ChatGPT-User`** — crawler en temps réel pour la fonctionnalité de browsing dans ChatGPT (distinct de GPTBot)
- [ ] **`User-agent: ClaudeBot`** — crawler Anthropic pour entraînement
- [ ] **`User-agent: Claude-Web`** — crawler Anthropic en temps réel pour Claude.ai
- [ ] **`User-agent: Google-Extended`** — opt-out du training de Bard/Gemini sans affecter Google Search indexing
- [ ] **`User-agent: PerplexityBot`** — crawler Perplexity
- [ ] **`User-agent: Applebot-Extended`** — opt-out d'Apple Intelligence training (Apple Search indexing continue)
- [ ] **`User-agent: CCBot`** — Common Crawl, utilisé par de nombreux datasets d'entraînement
- [ ] **`User-agent: Bytespider`** — TikTok/ByteDance crawler (entraînement)
- [ ] Cohérence : si le site veut être cité dans AI Overview / Perplexity mais pas servir de training data, autoriser `*-User` / `*-Web` (real-time) et bloquer les `*-Bot` (training)

**Exemple de stratégie « citation oui, training non »** :

```
# Search engines : autorisation pleine
User-agent: Googlebot
Allow: /

User-agent: Bingbot
Allow: /

# Real-time AI fetching : autorisation (pour apparaître dans AI Overview, Perplexity citations)
User-agent: ChatGPT-User
Allow: /

User-agent: Claude-Web
Allow: /

User-agent: PerplexityBot
Allow: /

# AI training : opt-out
User-agent: GPTBot
Disallow: /

User-agent: ClaudeBot
Disallow: /

User-agent: Google-Extended
Disallow: /

User-agent: Applebot-Extended
Disallow: /

User-agent: CCBot
Disallow: /

Sitemap: https://example.com/sitemap.xml
```

## `llms.txt` / `llms-full.txt` (standard émergent)

Le standard `llms.txt` proposé par [llmstxt.org](https://llmstxt.org/) fournit aux crawlers LLM une vue curée du site optimisée pour le contexte LLM (Markdown au lieu de HTML, structure claire, pas de chrome / nav / footer).

- [ ] **`/llms.txt`** présent à la racine ? Format Markdown, structure : H1 = nom du site, paragraphe = description, sections H2 = catégories de contenu avec liens
- [ ] **`/llms-full.txt`** présent ? Version longue avec contenu inliné des pages clés (utile si le site est petit et compact)
- [ ] Cohérence entre `llms.txt` et le sitemap : les pages importantes du sitemap apparaissent dans `llms.txt`
- [ ] URLs absolues dans `llms.txt`
- [ ] Mis à jour à la même cadence que le sitemap (sinon : stale info dans les réponses LLM)

**Standard encore en adoption (2026-Q2)** : pas universellement reconnu, mais l'absence n'a aucun coût et la présence aide les LLMs qui le respectent (Anthropic, Mistral, certains crawlers tiers).

## Contenu optimisé pour AI Overview / SGE / Perplexity citations

Les AI Overviews extraient des réponses synthétisées à partir de plusieurs sources citées. Pour maximiser la probabilité d'être cité :

- [ ] **Question-answer structure** : les pages explicatives commencent par une réponse synthétique de 50-100 mots avant le détail (les LLMs extraient le premier paragraphe direct)
- [ ] **Définitions claires** des concepts en début de section (les LLMs préfèrent les blocs définitionnels)
- [ ] **Listes à puces structurées** sur les questions « comment / quoi / pourquoi » (extraction LLM-friendly)
- [ ] **Sources citées inline** quand le site reprend des faits externes (renforce le signal d'authoritativeness)
- [ ] **`Article` JSON-LD avec `author` étendu** (voir `structured-data.md` § E-E-A-T) — crucial pour la confiance LLM
- [ ] **Date `dateModified` cohérente** avec la fraîcheur réelle du contenu (les LLMs préfèrent les sources récentes)
- [ ] **Tableaux et données structurées** pour les comparaisons (les AI Overviews citent souvent des tableaux)
- [ ] **Pas de contenu uniquement visuel** sans alternative textuelle (les LLMs ne lisent pas les images)

## E-E-A-T pour AI search

Les LLMs et AI Overviews s'appuient sur les mêmes signaux E-E-A-T que Google Search depuis 2022. Voir aussi `structured-data.md` § E-E-A-T expanded Author markup.

- [ ] **Page « À propos » détaillée** avec qualifications de l'équipe
- [ ] **Pages auteur** individuelles pour chaque contributeur (URL stable, photo, bio, lien vers les articles)
- [ ] **Mentions et liens externes vers les auteurs** (Wikipedia, profils universitaires, publications) — signal d'authoritativeness fort
- [ ] **`sameAs` dans le JSON-LD `Person`** pointant vers les profils sociaux et académiques de l'auteur
- [ ] **Mises à jour datées** sur le contenu (`dateModified` + bannière visible « Last updated: ... »)
- [ ] **Disclaimers** sur les contenus YMYL (Your Money Your Life — santé, finance, droit) : « Cet article ne remplace pas un avis professionnel »

## Anti-patterns AI-search

- 🚨 **Bloquer tous les `*Bot` AI sans réfléchir** — coupe l'opportunité d'apparaître dans AI Overview / Perplexity citations. Le bon arbitrage est par catégorie (training oui/non vs real-time oui/non).
- 🚨 **`llms.txt` désaligné avec le sitemap** — vaut mieux ne pas avoir `llms.txt` que d'en avoir un stale qui pollue le contexte LLM.
- 🚨 **Contenu généré par AI sans valeur ajoutée** publié à la chaîne — Google et les LLMs l'identifient et l'ignorent ou pénalisent. Le test : « cet article apporte-t-il quelque chose qu'un LLM ne peut pas générer à partir du prompt seul ? ».
- 🚨 **Author en simple string** dans `Article` JSON-LD → voir F3 de l'audit auto + section dédiée dans `structured-data.md`.
- 🚨 **Pas de `dateModified`** sur des articles « evergreen » qui sont en fait mis à jour silencieusement → les LLMs ne savent pas que le contenu est frais.

## Outils de validation

- **Inspector AI Overview** (Google Search Console → URL Inspection) — voir si l'URL est éligible aux AI Overviews
- **Schema.org Validator** — valider les Author/Person expansés
- **Perplexity Source Inspector** — voir quelles sources Perplexity cite pour une requête
- **Diffbot LLM Knowledge Graph** — vérifier que les entités du site sont correctement identifiées

## Cross-références internes

- **Structured data E-E-A-T** : voir `structured-data.md` § "Sites éditoriaux / blog" et § "Pièges fréquents" — l'expansion Author/Person est documentée là.
- **Content quality pour AI** : voir `content-seo.md` § "Intention de recherche" — les LLMs s'appuient sur le match intention/contenu encore plus que Google Search classique.
