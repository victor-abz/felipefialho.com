---
title: 'Stack com AI que vai elevar seu trampo como dev'
date: 2026-02-05 00:00:01
description: 'Usando o Claude Code como se tivesse um time de especialistas do lado"
image: /assets/2025-08-20-cover.jpg
tags: ['ai', 'front-end', 'programação']
---

Nos últimos meses, usando o [Claude Code](https://claude.ai/code) em projetos reais, parei de discutir se "AI escreve código ruim" e voltei pra pergunta que realmente importa: como usar o ferramental atual pra deixar meu desenvolvimento mais eficiente e escalável?

O resultado foi uma combinação que não substitui dev nenhum, mas te faz desenvolver como se tivesse um time de especialistas do lado.

## Stack (Front-end)

- [TanStack Query](https://tanstack.com/query/latest)
- [TypeScript](https://www.typescriptlang.org/)
- [Next.js](https://nextjs.org/)
- [ShadCN](https://ui.shadcn.com/)
- [React](https://react.dev/)

Essa combinação não é por acaso. LLMs reconhecem nativamente os padrões do Next.js, entendem as convenções do TypeScript e sabem trabalhar com os componentes do ShadCN sem necessidade de explicações detalhadas. Eles foram treinados com milhões de linhas de código dessa stack.

Resultado? Menos tempo corrigindo código gerado e mais tempo focado em arquitetura e features. A produtividade não vem de gerar código cego, vem de gerar código que você confia e que funciona.

## Skills vs Rules

### Skills: O DNA do seu projeto

Skills são conhecimentos específicos do seu projeto: suas regras de design system, o pattern específico de Server Components que seu time usa, as convenções de nomenclatura do TypeScript em sua base de código e a estrutura de pastas customizada. É tudo que faz seu projeto único.

A mágica das Skills é que são chamadas sob demanda. Trabalhando em autenticação? O Claude carrega só as Skills de auth. Mexendo no design system? Só as Skills de UI entram no contexto. Isso economiza tokens massivamente e mantém o foco onde importa.

**Exemplo de Skill de autenticação:**
- Sempre use NextAuth.js para autenticação
- Sessions devem ser validadas usando middleware do Next.js
- Tokens JWT devem ter expiração de 7 dias
- Refresh tokens devem ser armazenados em httpOnly cookies

### Rules: As boas práticas universais

Rules são universais e sempre ativas: SOLID, clean code, padrões [WCAG](https://www.w3.org/WAI/WCAG21/quickref/) de acessibilidade, otimizações de [Core Web Vitals](https://web.dev/vitals/), proteção contra [OWASP Top 10](https://owasp.org/www-project-top-ten/). São as boas práticas que valem para qualquer projeto e precisam estar presentes o tempo todo.

Quando você separa assim, o Claude consegue priorizar: primeiro, seguem suas Skills (porque são específicas e contextuais), depois, aplica Rules (porque são gerais). Isso evita conflitos e mantém consistência sem desperdiçar tokens.

## MCPs que fazem diferença

[Model Context Protocol (MCP)](https://modelcontextprotocol.io/) é o padrão da Anthropic para conectar o Claude com ferramentas externas. Aqui estão os MCPs essenciais:

### Filesystem

O [MCP Filesystem](https://www.npmjs.com/package/@modelcontextprotocol/server-filesystem) permite que o Claude leia, crie e modifique arquivos do seu projeto diretamente. Ele consegue fazer refactors em múltiplos arquivos, criar novas features completas, ou analisar toda sua codebase.

**Casos de uso:**
- Refatorar componentes React em 10+ arquivos simultaneamente
- Criar estrutura completa de feature (componentes, hooks, types, testes)
- Analisar dependencies entre módulos

### Context7

Busca documentação atualizada e específica por versão de bibliotecas e frameworks, injetando diretamente no contexto do Claude. Elimina respostas baseadas em APIs deprecadas ou métodos que não existem mais.

Quando você pergunta sobre App Router no Next.js 15, o [Context7](https://context7.com/) puxa os docs da versão exata que você usa, não uma mistura de Pages Router e App Router de versões antigas.

### Server Memory

É crucial; mantém decisões arquiteturais entre sessões, lembra as preferências do projeto e guarda aprendizados de erros anteriores. Seu contexto não some quando você fecha o Claude.

O [Server Memory](https://www.npmjs.com/package/@modelcontextprotocol/server-memory) funciona como memória de longo prazo. Se você decidiu que "nunca usar any no TypeScript, sempre unknown", o Claude lembra disso nas próximas sessões sem você precisar repetir.

### Playwright Extension

Automatiza testes end-to-end direto do ambiente, rodando cenários completos e validando comportamentos.

Integração com [Playwright](https://playwright.dev/) permite que o Claude:
- Rode testes e2e automaticamente
- Identifique flakiness em testes
- Sugira melhorias baseadas em failures

### Sequential Thinking

Estrutura o raciocínio do Claude em etapas claras, evitando soluções apressadas e garantindo análise completa antes de qualquer implementação.

Em vez de pular direto pra implementação, o [Sequential Thinking](modelcontextprotocol/server-sequential-thinking) força o Claude a:
1. Entender o problema completamente
2. Considerar múltiplas abordagens
3. Avaliar trade-offs
4. Só então propor solução

## Workflow

### Modo Plan com Opus

Antes de codar qualquer feature, deixo o [Opus 4.5](https://www.anthropic.com/news/claude-opus-4-5) derivar a estratégia completa: análise de requisitos, quebra em tasks menores, identificação de riscos e trade-offs. Ele pensa em escalabilidade, manutenibilidade, e impacto no resto do sistema.

**Exemplo de prompt para Plan:**
"Preciso adicionar feature de notificações em tempo real. Analise a arquitetura atual e proponha estratégia considerando escalabilidade, custo de infra, e experiência do usuário"

### Modo Agent com Sonnet

Para implementação rápida e eficiente. [Sonnet 4.5](https://www.anthropic.com/news/claude-4-5-sonnet) é mais rápido e econômico, mantendo qualidade excelente. Se você tem tokens de sobra, Opus também brilha aqui, mas Sonnet entrega 90% da qualidade por uma fração do custo.

### Plans commitados

Cada plan só avança quando está realmente concluído. Nada de deixar pontas soltas; se alguma decisão ficar pra depois, isso precisa ficar explícito. Tudo documentado, tudo validado, tudo pronto para executar.

**Estrutura de um plan completo:**

```md
- [ ] Requisitos funcionais mapeados
- [ ] Requisitos não-funcionais definidos (performance, segurança)
- [ ] Dependências identificadas
- [ ] Riscos mapeados com mitigações
- [ ] Tasks priorizadas
- [ ] Critérios de aceite claros
```

### ADRs

[Architecture Decision Records](https://adr.github.io/) podem ser gerados com Skills específicas de ADR. Por que escolhemos Server Components aqui? Por que optamos por React Query em vez de SWR? Tudo fica registrado, criando memória institucional do projeto. Isso é crucial para manter a consistência e evitar surpresas no futuro.

```md
**Exemplo de ADR:**

**Título:** Uso de Server Components para Dashboard

**Contexto:** Dashboard precisa exibir dados de múltiplas APIs com refresh a cada 30s

**Decisão:** Server Components com streaming + React Query para updates

**Consequências:**
- Melhora SEO e tempo de carregamento inicial
- Aumenta complexidade de state management
- Requer Next.js 14+
```

## Subagentes especializados

Subagents são "personas" focadas que você cria manualmente no Claude Code, via `/agents` ou em [arquivos de configuração](https://docs.anthropic.com/en/docs/agents/overview). Cada um tem expertise e contexto específicos em uma área. Requer setup inicial, mas o retorno compensa.

### Planner

Quebra features complexas em tasks executáveis, identifica dependências entre componentes, sugere ordem de implementação que minimiza bloqueios, e antecipa problemas antes de você começar a codar.

**Prompt de configuração:**
"Você é um planejador técnico especializado em arquitetura de front-end. Seu objetivo é quebrar features em tasks executáveis, identificar dependências, e antecipar riscos técnicos"

### Code Review

Vai além de linting, encontra problemas de lógica que testes não pegariam, identifica trade-offs de performance (aquele map dentro de map que vai explodir com dados reais), e vulnerabilidades sutis que passariam despercebidas.

O subagent de Code Review é configurado para focar em:
- Race conditions em async code
- Memory leaks em useEffect
- N+1 queries ocultos
- Vulnerabilidades de segurança (XSS, CSRF)

### Refactor

Analisa código existente com olhar para o futuro, propõe abstrações que vão facilitar próximas features, identifica duplicações não óbvias, sugere patterns que melhoram testabilidade.

**Foco do Refactor agent:**
- Identificar código duplicado semanticamente (não só textualmente)
- Propor abstrações que facilitam extensão
- Melhorar testabilidade sem quebrar funcionalidade

### E2E

Cria planos de teste end-to-end completos para Playwright, pensa em edge cases que você esqueceria (o que acontece se o usuário apertar Enter antes da API responder?), cobertura de fluxos críticos, e cenários de erro.

**Exemplo de cobertura:**
- Fluxos felizes (cadastro → login → ação principal)
- Edge cases (timeout de API, perda de conexão)
- Casos de erro (validações, permissões)
- Estados intermediários (loading, retry)

### Segurança

Faz análise profunda de brechas: SQL injection, XSS, CSRF, vazamento de dados sensíveis, configurações inseguras, dependências vulneráveis.

O agent de Segurança verifica:
- Inputs não sanitizados (XSS)
- Queries diretas no banco (SQL injection)
- Tokens expostos em client-side
- Dependências com CVEs conhecidos
- Headers de segurança faltando

## Impacto real

Essa estrutura não elimina a necessidade de pensar, ela amplifica sua capacidade de execução. Você continua sendo o arquiteto, mas agora tem um time de especialistas à disposição 24/7 pra ajudar.

O código gerado tem qualidade consistente. Os reviews pegam coisas que passariam. As refatorações seguem princípios sólidos. E tudo fica documentado para quem entrar no projeto depois.

Código ruim existe desde muito antes de LLMs surgirem, já tinha legado monstruoso quando a gente ainda gerava bundles com jQuery e script tags. A diferença é que agora dá pra usar AI pra atuar em larga escala em cima de uma stack bem pensada e estruturada.

E vale lembrar que independentemente do ferramental que você usa, o responsável por cada entrega segue sendo VOCÊ.

## Recursos adicionais:
- [Documentação oficial do Claude Code](https://docs.anthropic.com/en/docs/claude-code)
- [MCP Server Registry](https://github.com/modelcontextprotocol/servers)
- [ADR Templates](https://github.com/joelparkerhenderson/architecture-decision-record)

