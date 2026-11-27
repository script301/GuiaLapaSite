# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Users

**Primário:** Turistas e visitantes de Bom Jesus da Lapa (BA) — romeiros, turistas religiosos e de lazer — que chegam à cidade e precisam encontrar estabelecimentos comerciais, horários de funcionamento, localização no mapa e informações confiáveis para tomar decisões rápidas (onde comer, onde ficar, o que visitar, serviços essenciais).

**Situação:** Chegada a uma cidade desconhecida, muitas vezes com conectividade instável, pressa ou necessidade específica (ex.: farmácia aberta agora, restaurante perto do Santuário, hospedagem com vaga).

**Job to be done:** "Encontrar o que preciso em Bom Jesus da Lapa agora, com informação confiável e sem depender de plataformas que vendem meus dados."

**Outros públicos (secundários):** Moradores locais que consultam o guia no dia a dia; comerciantes/empreendedores que querem aparecer no guia e gerenciar sua presença.

## Product Purpose

O Guia Lapa é um guia comercial e turístico **estático, leve e centrado em privacidade** para Bom Jesus da Lapa. Ele existe para:

- Substituir buscas genéricas (Google Maps, listas genéricas) por **curadoria local verificada** — estabelecimentos confirmados, horários reais, relevância para o contexto da cidade (romaria, Santuário, comércio do centro).
- Oferecer **mapa interativo (Leaflet/OpenStreetMap) + avaliações/perguntas nativas** sem depender de plataformas terceiras que rastreiam usuários.
- Entregar **conformidade LGPD e WCAG 2.2 AA por padrão** — zero venda de dados, consentimento granular, acessibilidade real — como diferencial de confiança.

**Sucesso** = visitante encontra o estabelecimento certo na primeira busca, comerciante vê retorno real de visibilidade, e nenhum dado pessoal vaza ou é monetizado.

## Positioning

**Mecanismo diferencial:** Curadoria local + confiança (privacidade como feature, não compliance).

Um mapa genérico ou diretório não tem:
- Verificação humana de cada estabelecimento listado (horários, endereço, categoria, relevância local)
- Contexto de romaria/turismo religioso (o que abre cedo para romeiros, onde fica a procissão, comércio do entorno do Santuário)
- Privacidade real: sem Google Analytics por padrão, sem pixels de publicidade, cookies de estatística e mapas só com consentimento explícito, dados de geolocalização processados só no navegador
- Avaliações/perguntas nativas sem lock-in de plataforma (o comerciante responde direto no guia)

**Posição no mercado:** "O guia que quem vive em Lapa faria para quem visita Lapa — e que não te vigia."

## Operating Context

- **Ambiente:** Web estática servida via Vercel (primário) e Netlify (espelho de headers via `_headers`).
- **Stack:** HTML5 semântico, CSS custom properties (light/dark via `prefers-color-scheme`), JS vanilla (ES modules). Sem build step, sem framework.
- **Backend:** Supabase (PostgreSQL, Auth, RPC) — chave pública injetada via `window.SUPABASE_KEY` no `index.html` (variável de ambiente no Vercel). Chave *publishable* `sb_publishable_-ZwxP-TW0ZJd199E_dNjDA_HQ5ZtXBo` recomendada; *legacy anon key* desativada e expurgada do histórico git.
- **Mapas:** Leaflet + tiles OpenStreetMap (`https://*.tile.openstreetmap.org`). Carregamento **lazy e condicionado ao consentimento** (categoria "funcionalidades") — script `type="text/plain" data-categoria-consentimento="funcionalidades"` substituído dinamicamente pelo `consentimento.js`.
- **Consentimento:** Banner próprio (`consentimento.js` + `consentimento.css`) com Google Consent Mode v2 (padrão `denied`), categorias: `necessario` (sempre), `estatistica`, `funcionalidades`. Preferências em `localStorage` (365 dias). Reabertura via botão "Preferências de cookies" no rodapé.
- **Segurança (headers):** HSTS preload (1 ano), `X-Frame-Options: DENY`, `X-Content-Type-Options: nosniff`, `Referrer-Policy: strict-origin-when-cross-origin`, `Permissions-Policy: geolocation=(self), camera=(), microphone=(), payment=()`, CSP restritivo (`connect-src` apenas Supabase + OSM tiles + Mapbox events).
- **Acessibilidade:** WCAG 2.2 AA — contraste 4.5:1 (`--foco` theme-aware), skip link, foco visível, navegação teclado, `lang="pt-BR"`, `alt` descritivo, zoom liberado.
- **Documentos legais:** Política de Privacidade, Termos de Uso, Política de Cookies, Declaração de Acessibilidade — todos linkados no rodapé e navegáveis.
- **Identificação do controlador:** Bruno Moura da Silva Oliveira, CPF 119.494.764-50, Rua L, 69, Bairro São João, Bom Jesus da Lapa – BA, CEP 47600-000. Canal LGPD: `bmourasilvaoliveira@gmail.com` / (77) 99965-9126.

## Capabilities and Constraints

**Funcionalidades confirmadas:**
- Listagem de estabelecimentos com busca/filtro por categoria
- Mapa interativo (Leaflet/OSM) com geolocalização do usuário (apenas no browser, não armazenada no servidor)
- Sistema de avaliações e perguntas/respostas (Supabase RPC + RLS)
- Consentimento granular de cookies (necessário / estatística / funcionalidades)
- Tema claro/escuro persistido
- PWA-ready (manifest, ícones em `marca/`)

**Restrições técnicas:**
- Sem server-side rendering, sem Node.js no deploy — tudo estático + Supabase client-side
- CSP impede `eval()`, `unsafe-eval`, inline scripts só onde explicitamente permitido (`'unsafe-inline'` em `script-src` e `style-src` por legado do consentimento e tema)
- Geolocalização: `Permissions-Policy: geolocation=(self)` + processamento só no cliente
- Chaves sensíveis **nunca** no repositório (`.gitignore`: `config.js`, `.env*`). Histórico limpo via `git filter-branch`.

**Fatos explícitos ainda não decididos:**
- Modelo de monetização (se houver) — atualmente gratuito para usuário e comerciante
- Escala de estabelecimentos (atual: curado manualmente; futuro: submissão moderada?)
- Integração com WhatsApp Business API para contato direto (hoje: link `wa.me`)

## Brand Commitments

- **Nome:** Guia Lapa (consolidado)
- **Logo:** Assets em `marca/` — `logo-192.png` (PWA), `favicon.ico`, `apple-touch-icon.png`, `icone_app.png`, `logo_nome.png`
- **Voz/Tom:** Direto, acolhedor, sem jargão, transparente. "Feito por quem conhece Lapa para quem visita Lapa."
- **Personalidade:** Confiável, local, respeitoso da privacidade, útil acima de bonito.
- **Identidade visual:** CSS custom properties (`--azul-profundo`, `--azul-ceu`, `--ouro`, `--foco` theme-aware). Tipografia: Inter (UI) + Fraunces (display). Cores testadas para AA em ambos temas.
- **Compromisso legal:** LGPD (Art. 7, 18, 33, 41), CDC (Art. 101 foro consumidor), Marco Civil (Art. 15 guarda logs 6 meses), LBI/WCAG 2.2 AA / NBR 17225.

## Evidence on Hand

- **Código-fonte completo** (este repositório) — páginas: `index.html`, `politica-de-privacidade.html`, `termos-de-uso.html`, `politica-de-cookies.html`, `declaracao-de-acessibilidade.html`
- **Headers de segurança** — `vercel.json` (Vercel) + `_headers` (Netlify mirror)
- **Consentimento funcional** — `consentimento.js`, `consentimento.css` (testado: bloqueia GA/Mapas até aceite, reabre via rodapé)
- **Acessibilidade validada** — contraste `--foco` 4.54:1 AA (light/dark), skip link, foco visível, navegação teclado, `alt` em todas as imagens
- **Documentos legais** — LGPD-completos com controlador, DPO (canal), bases legais por finalidade, transferência internacional (Supabase EUA, cláusulas padrão), retenção (6 meses logs, 12 meses cookies, 26 meses GA), direitos do titular com canal e prazo 15 dias
- **Auditoria jurídica** — `AUDITORIA-JURIDICA.md` (17 itens, scorecard, marcadores `[[PREENCHER]]` para licenças de imagens)
- **Assets visuais** — `marca/` (5 arquivos; licenças a confirmar — ver `[[PREENCHER: licenças imagens marca/]]`)
- **Deploy funcional** — Vercel + Netlify headers espelhados

**Ausências que o trabalho futuro não deve fabricar:**
- Depoimentos/testemunhos de usuários (não existem; não inventar)
- Métricas de conversão/engajamento (não rastreadas por padrão)
- Certificações formais de acessibilidade (auto-declaração na Declaração de Acessibilidade)
- Parcerias oficiais com Prefeitura/Santuário (não confirmadas)

## Product Principles

1. **Privacidade por arquitetura, não por promessa** — Se o código não envia, não vaza. Consentimento real (opt-in), default denied, sem dark patterns.
2. **Curadoria humana vence algoritmo genérico** — Um estabelecimento verificado por quem mora lá vale mais que 100 resultados de API.
3. **Leveza é acessibilidade** — HTML estático, CSS nativo, JS mínimo, zero dependências de runtime. Carrega em 3G, funciona sem JS (conteúdo essencial), respeita `prefers-reduced-motion`.
4. **Confiança se conquista em detalhes** — Contraste AA nos dois temas, foco visível, skip link, alt descritivo, headers de segurança, documentos legais honestos e linkados.
5. **Local é o diferencial, não commodity** — Informação que só quem vive em Lapa sabe (horários de romaria, comércio do entorno do Santuário, rotas de procissão) é o moat.

## Accessibility & Inclusion

- **Padrão requerido:** WCAG 2.2 AA (conformidade auto-declarada na `declaracao-de-acessibilidade.html`).
- **Necessidades conhecidas:** Usuários com baixa visão (contraste 4.5:1, zoom até 200%), navegação só por teclado (foco visível, skip link, ordem lógica), leitores de tela (`lang="pt-BR"`, `alt` descritivo, ARIA labels em ícones/botões), vestibular/fotosensibilidade (`prefers-reduced-motion` respeitado, sem auto-play).
- **Gaps a monitorar:** Testes com usuários reais com deficiência (não realizados), auditoria externa formal (pendente).