# LC Soluções em Climatização — Site estático

Versão **HTML estático puro** (sem React/build), pronta para deploy no Vercel.
Convertida a partir do `LC Solucoes.dc.html` para SEO e velocidade.

## Estrutura
```
dist/
├── index.html        ← página única (todo conteúdo no HTML, sem JS de framework)
├── vercel.json       ← cache de assets + headers de segurança
├── robots.txt        ← libera indexação + aponta sitemap
├── sitemap.xml       ← 1 URL (home)
└── assets/           ← imagens, favicons, logo
```

## Deploy no Vercel

### Opção A — via dashboard (mais simples)
1. Acesse vercel.com → **Add New… → Project**
2. Importe o repositório OU arraste a pasta `dist/`
3. Em **Framework Preset** escolha **Other** (é estático)
4. **Output Directory**: deixe vazio (a raiz já é o site) ou `.`
5. Deploy.

### Opção B — via CLI
```bash
npm i -g vercel
cd dist
vercel --prod
```

## O que foi feito (SEO + Velocidade)

**SEO**
- Conteúdo 100% no HTML (Google lê sem executar JS) — antes era client-side React
- `<title>`, meta description, canonical, Open Graph, theme-color
- JSON-LD: `LocalBusiness` (com nota, telefone, geo, serviços) + `FAQPage`
- 1 único `<h1>`, hierarquia h2/h3 correta, `lang="pt-BR"`
- `robots.txt` + `sitemap.xml`
- Palavra-chave local "ar-condicionado em Goiânia" nos títulos

**Velocidade**
- Zero JavaScript de framework (React removido) — só ~1KB de JS próprio (menu + contadores)
- Fontes carregadas de forma não-bloqueante (`media=print onload`)
- `loading="lazy"` nas imagens abaixo da dobra; `fetchpriority="high"` na imagem do hero
- `width`/`height` em todas as imagens (evita layout shift / CLS)
- FAQ com `<details>` nativo (acessível, indexável, sem JS)
- Cache de 1 ano para `/assets/*` (vercel.json)
- Headers de segurança (nosniff, frame-options, referrer-policy)
- Assets órfãos removidos (economia de ~2,5MB)

## ⚠️ Otimização de imagem pendente (recomendado)

Estas imagens ainda estão grandes. Converter para **WebP** corta ~70% do peso e melhora muito o PageSpeed:

| Imagem | Tamanho atual | Ação |
|---|---|---|
| `assets/sobre-equipe.png` | ~662 KB | converter p/ WebP (~150 KB) |
| `assets/hero-bg.png` | ~633 KB | converter p/ WebP (~150 KB) |
| `assets/logo-horizontal.png` | ~290 KB | re-exportar otimizado (~30 KB) |

Como converter (escolha um):
- **Squoosh** (online, grátis): https://squoosh.app — abra, escolha WebP, qualidade ~80, baixe.
- **CLI** (se tiver): `cwebp -q 80 hero-bg.png -o hero-bg.webp`

Depois, no `index.html`, troque `.png` por `.webp` nos `src` correspondentes
(ou use `<picture>` com fallback). Posso fazer isso quando os WebP existirem.

## Falta criar (opcional)
- `assets/og-image.png` — imagem 1200×630 para compartilhamento no WhatsApp/redes.
  Hoje o Open Graph aponta para `hero-bg.png` como fallback. Crie uma arte dedicada
  (logo + headline) para ficar profissional ao compartilhar o link.

## Domínio
As URLs canônicas usam `https://lcsolucoes.com.br/`. Se o domínio final for outro,
ajuste em `index.html` (canonical + og:url + JSON-LD) e em `sitemap.xml`/`robots.txt`.
