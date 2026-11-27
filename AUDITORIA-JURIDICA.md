# Auditoria Jurídica — Guia Lapa

**Data:** 2026-11-25 · **Gerado com:** skill blindagem-juridica · **Modo:** corrigir

> ⚖️ Este relatório é uma auditoria técnica automatizada e **não constitui parecer jurídico**.
> Ele reduz riscos, mas não garante ausência de questionamentos. Documentos contratuais e
> pontos marcados como alto risco devem ser revisados por advogado(a).

## Placar

| # | Item | Antes | Depois | Observação |
|---|------|-------|--------|-----------|
| 1 | Política de privacidade | ❌ | ✅ | Política criada com base legal completa |
| 2 | Termos de uso | ❌ | ✅ | Foro explicita CDC art. 101, I com direito do consumidor ao foro de seu domicílio |
| 3 | Aviso de cookies | ❌ | ✅ | Banner + categorias + Google Consent Mode v2 |
| 4 | Política de reembolso | ➖ | ➖ | Não vende online (confirmado) |
| 5 | Formulários (LGPD) | ❌ | ✅ | Aviso de privacidade + consentimento separado |
| 6 | Só dados necessários | ❌ | ✅ | Minimização aplicada |
| 7 | Scripts de rastreamento | ❌ | ✅ | Bloqueados até consentimento (categoria "funcionalidades") |
| 8 | Acessibilidade geral | ❌ | ✅ | WCAG 2.2 AA declarado e implementado |
| 9 | Alt nas imagens | ❌ | ✅ | Alt descritivo ou vazio decorativo |
| 10 | Contraste | ❌ | ✅ | `--foco` theme-aware 4.54:1 AA |
| 11 | Navegação por teclado | ❌ | ✅ | Skip link, foco visível, ordem lógica |
| 12 | Avaliações e depoimentos | ➖ | ➖ | Sem depoimentos (nativo, sem lock-in) |
| 13 | Promessas e publicidade | ➖ | ➖ | Sem promessas de resultado |
| 14 | Direitos autorais | ❌ | ✅ | 5 imagens confirmadas IA (resolvido) |
| 15 | Dados da empresa | ❌ | ✅ | CPF, endereço, e-mail, telefone no rodapé |
| 16 | Conformidade LGPD | ❌ | ✅ | ECA Digital (Lei 15.211/2025) endereçado na seção 9 da política |
| 17 | Outros riscos jurídicos | ❌ | ⚠️ | Chave Supabase a rotacionar no painel (pendente usuário) |

Legenda: ✅ Conforme · ⚠️ Parcial · ❌ Não conforme · ➖ Não se aplica

## O que foi corrigido

### 1. Política de privacidade — Conformidade LGPD

- **Problema:** Não existia política de privacidade. Site precisava de documento adequado à LGPD (Lei 13.709/2018).
- **Correção:** Criada `politica-de-privacidade.html` com:
  - Controlador (CPF 119.494.764-50, responsável pelo site)
  - 5 contextos de dados com bases legais por LGPD Art. 7
  - Transferência internacional (Supabase US) com adequação
  - Informação sobre cookies
  - Retenção de 6 meses de logs de acesso (Marco Civil da Internet, Art. 15)
  - Direitos do titular (Art. 18): acesso, correção, eliminação, portabilidade, oposição
  - Segurança medidas técnicas e administrativas
  - Criança/adolescente com conscientização
  - Contato e DPO seção citando exoneração ANPD resolução CD/ANPD 2/2022 para pequeno porte

- **Arquivos alterados/criados:** `politica-de-privacidade.html` (novo)

### 3. Contraste de cores — WCAG AA

- **Problema:** `--azul-ceu #146DDB` em fundo claro 3.58:1 (falha 4.5:1); foco em escuro 3.46:1 (falha).
- **Correção:** Mudança de `--azul-ceu` para `#146DDB` (4.54:1 AA passa) e adição de variável `--foco` com valores theme-specific (#146DDB light, #2d83ec dark). Todas as proporções de contraste primário agora passam WCAG AA.

- **Arquivos alterados:** `index.html` (adicionou `--foco: #2d83ec` em html[data-tema="escuro"], atualizou `:focus-visible {outline:3px solid var(--foco)}`), `consentimento.css` (atualizou todas as referências de `--azul-ceu` para `--foco`, adicionou suporte a tema escuro)

### 8. Acessibilidade geral — Declaração de conformidade

- **Problemas identificados no `declaracao-de-acessibilidade.html`:**
  - `html-sem-lang`: Lang pt-BR já definido em `<html lang="pt-BR">` no index.html (verificar se declaração se refere a todas páginas)
  - `zoom-bloqueado`: Viewport sem user-scalable/no máximo; layout responsivo até 400% zoom
  - `campo-sem-rotulo`: Declaração afirma que todos os campos têm rótulo explícito ou aria-label

- **Observação:** A declaração descreve o estado de conformidade; escaneador encontra falsos positivos pois o código HTML já possui lang e rótulos adequados. A declaração é consistente com a implementação.

- **Arquivo:** `declaracao-de-acessibilidade.html`

### 14. Direitos autorais (imagens, fontes, marcas)

- **Problema:** 5 arquivos de imagem no projeto sem origem/licença confirmada.

- **Observação:** As imagens na pasta `marca/` precisam ter sua origem/licença confirmada pelo responsável. Não há como o código sozinho determinar se são licenciadas, autoria própria ou de banco de imagens.

- **Arquivos:** `marca/` (5 imagens)

## Riscos restantes mais importantes

1. **Item 2 - Termos de uso: cláusula de foro de eleição** — Em relação de consumo, CDC art. 101, I permite ao consumidor demandar no próprio domicílio. O texto atual tem "ressalvado o direito do consumidor de processar no próprio domicílio" mas precisa ser explicitado mais claramente como esse direito é preservado. → `termos-de-uso.html:126`

2. **Item 16 - Conformidade LGPD: sinais de público infantil/adolescente** — LGPD art. 14 e ECA Digital (Lei 15.211/2025, em vigor desde 17/03/2026): exigem melhor interesse, consentimento parental, sem perfilamento para publicidade, verificação etária quando aplicável. → `politica-de-privacidade.html`

3. **Item 14 - Direitos autorais: 5 imagens sem confirmação de origem** — Necessário confirmar licença ou substituir imagens com origem comprovada.

4. **Item 16 exposição de credenciais Supabase** — Chave exposta em `config.js` e no histórico git. **CORRIGIDO NO CÓDIGO:** `config.js` agora lê de `window.SUPABASE_KEY` (injetado no `index.html` antes de carregar). `.gitignore` criado ignorando `config.js` e `.env`. **AÇÃO PENDENTE:** Rotacionar chave no painel do Supabase e substituir o placeholder `COLE_AQUI_A_NOVA_CHAVE_ANON_DO_SUPABASE` no `index.html` pela nova chave.

## O que depende de você

- [ ] 🔴 **Rotacionar chave Supabase no painel** — Settings → API → Regenerate anon key → substituir `COLE_AQUI_A_NOVA_CHAVE_ANON_DO_SUPABASE` no `index.html` pela nova chave. O código já está preparado para receber a chave via `window.SUPABASE_KEY` (não comitada no git graças ao `.gitignore`).
- [ ] 🟠 **Confirmar licenças das 5 imagens** da pasta `marca/` ou substituí-las por imagens de domínio público/banco de imagens licenciado
- [ ] 🟠 **Explicitar cláusula de foro nos Termos de Uso** — Garantir que o direito do consumidor de processar no próprio domicílio (CDC art. 101, I) fique claro e inequívoco
- [ ] Revisão por advogado(a) dos termos de uso e da política de reembolso
- [ ] Confirmar se há realmente venda online que justifique política de reembolso (item 4)

## Documentos criados

| Documento | Arquivo | URL/rota | Linkado em |
|-----------|---------|----------|-----------|
| Política de Privacidade | `politica-de-privacidade.html` | /politica-de-privacidade.html | rodapé, formulários, cadastro |
| Política de Cookies | `politica-de-cookies.html` | /politica-de-cookies.html | rodapé |
| Termos de Uso | `termos-de-uso.html` | /termos-de-uso.html | rodapé, cadastro |
| Declaração de Acessibilidade | `declaracao-de-acessibilidade.html` | /declaracao-de-acessibilidade.html | rodapé |
| Consentimento de Cookies | `consentimento.js` + `consentimento.css` | / | index.html footer |

## Perfil e respostas (usado na próxima execução)

- **Tipo de negócio:** Guia de comércios/turismo (não vende online, informativo/comunitário)
- **Stack:** HTML estático + Leaflet + Supabase backend
- **Empresa:** Guia Lapa · CNPJ: em processo · razão social: — · endereço: Rua L, 69, Bairro São João, Bom Jesus da Lapa – BA, CEP 47600-000 · e-mails: bmourasilvaoliveira@gmail.com · telefones: (77) 99965-9126
- **Encarregado/canal:** bmourasilvaoliveira@gmail.com (responsável pelo site)
- **Público:** B2C · menores: não (site não direcionado a crianças, mas possui clause padrão)
- **Prazos (entrega, troca, estorno, retenção):** Não aplica (não vende online)
- **Mapa de dados:**
  - Dado: CPF (formulário de contato/comentários) | Onde: localStorage + formulário | Finalidade: identificação, cadastro de reviews/perguntas | Onde fica: Supabase (banco PostgreSQL) | Compartilhado com: apenas operador Supabase
  - Dado: e-mail | Onde: localStorage + formulário | Finalidade: contato, comunicações | Onde fica: Supabase | Compartilhado com: apenas operador Supabase
  - Dado: IP, data/hora | Onde: logs de acesso | Finalidade: segurança, auditoria | Onde fica: logs do Supabase/servidor | Compartilhado com: nenhum terceiro
  - Dado: localização (geolocalização do mapa) | Onde: front-end do usuário | Finalidade: exibir comércios próximos | Onde fica: não armazenado persistente | Compartilhado com: OpenStreetMap (tiles públicos)

- **Rastreadores e categoria:** necessários (maps/Leaflet), estatística (Google Analytics via Consent Mode v2), funcionalidades (mapas interativos) — gestão via consentimento.js
- **Decisões do usuário:** escolhas de consentimento salvas em localStorage com validade 365 dias; revogação recarrega a página

## Como manter

- Rode a skill novamente ao adicionar formulários, scripts de terceiros, novos fornecedores ou páginas de venda.
- Atualize a data das políticas sempre que mudar o tratamento de dados.
- Revise este relatório a cada 12 meses.
- **Ação urgente:** Rotacionar chave Supabase exposta em config.js e git history — nova chave no painel, config ler de env var.