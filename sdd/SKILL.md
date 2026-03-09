---
name: sdd
description: Spec-driven development.
---

# 📋 Specification-Driven Development (SDD) — AllDone

## Objetivo

Este documento define **como toda nova funcionalidade deve ser especificada** antes da implementação.
A especificação é o artefato primário — código é sua expressão. Não existe "vibe coding".

> **Regra de ouro**: Se a spec não descreve o cenário do usuário, os requisitos funcionais,
> e os critérios de sucesso — a funcionalidade **não está pronta para ser implementada.**

*Inspirado no [GitHub Spec Kit](https://github.com/github/spec-kit) — Specification-Driven Development.*

---

## Filosofia

- **Intent-first**: Especificações definem o "O QUÊ" antes do "COMO"
- **Refinamento iterativo**: Múltiplas etapas de revisão, não geração one-shot
- **Independência tecnológica**: Specs descrevem comportamento, não implementação
- **Rastreabilidade**: Toda decisão técnica é vinculada a um requisito funcional
- **Clarificação explícita**: Toda ambiguidade é resolvida ANTES de planejar
- **Select-first**: Se o dado já existe no sistema, o componente é `Select`/`Combobox`, nunca `Input text`
- **i18n-native**: Toda string visível ao usuário usa `t("key")`, documentada na spec

---

## Processo de Desenvolvimento

### Visão Geral das Fases

```
1. SPECIFY   → Especificação funcional (O QUÊ e POR QUÊ)
2. CLARIFY   → Esclarecimento de dúvidas com o stakeholder
3. PLAN      → Planejamento técnico (COMO)
4. TASKS     → Decomposição em tarefas atômicas
5. IMPLEMENT → Execução com verificação contínua
```

### Regras por Fase

| Fase | Quem lidera | Output | Regra |
|---|---|---|---|
| SPECIFY | AI + Stakeholder | `spec.md` | Foco no O QUÊ. Sem tech stack. Technology-agnostic. |
| CLARIFY | AI pergunta → Stakeholder responde | Seção `Clarifications` na spec | Toda ambiguidade deve ser resolvida ANTES de planejar |
| PLAN | AI | `plan.md` | Detalhar arquitetura, componentes, ordem de criação |
| TASKS | AI | `tasks.md` | 1 task = 1 entregável testável. Sem tasks vagas. |
| IMPLEMENT | AI | Código | Cada task deve atingir o DOD antes de avançar |

### Fluxo Visual

```
  Ideia/Pedido
       ↓
  ┌─────────┐
  │ SPECIFY │ → spec.md (O QUÊ)
  └────┬────┘
       ↓
  ┌─────────┐
  │ CLARIFY │ → Perguntas e respostas adicionadas à spec
  └────┬────┘
       ↓
  ┌─────────┐
  │  PLAN   │ → plan.md (COMO)
  └────┬────┘
       ↓
  ┌─────────┐
  │  TASKS  │ → tasks.md (EM QUE ORDEM)
  └────┬────┘
       ↓
  ┌───────────┐
  │ IMPLEMENT │ → Código (task por task, com DOD)
  └───────────┘
```

> **IMPORTANTE**: Nenhuma funcionalidade nova deve ser implementada sem antes ter sua spec criada e aprovada.

---

## Estrutura de Specs

### Organização de Arquivos

Cada feature é uma pasta numerada sequencialmente:

```
docs/specs/
├── 001-feature-name/
│   ├── spec.md              ← Especificação funcional
│   ├── plan.md              ← Plano técnico
│   └── tasks.md             ← Decomposição em tarefas
├── 002-another-feature/
│   ├── spec.md
│   ├── plan.md
│   └── tasks.md
└── ...
```

### Convenção de Nomenclatura

```
NNN-nome-descritivo-em-kebab-case/
```

| Parte | Regra | Exemplo |
|---|---|---|
| `NNN` | Número sequencial com 3 dígitos, zero-padded | `001`, `012`, `100` |
| `nome` | Kebab-case, máximo 5 palavras | `user-authentication`, `process-designer` |

### Para descobrir o próximo número

```bash
ls docs/specs/ | sort -n | tail -1
# Se o último é 003-xxx, o próximo é 004-yyy
```

---

## Template: spec.md

> Toda nova feature DEVE seguir este template.
> Salvar em: `docs/specs/NNN-feature-name/spec.md`

````markdown
# Feature: [Nome da Feature]

**Spec**: `NNN-feature-name`
**Criado em**: [DATA]
**Status**: Draft | In Review | Approved
**Input**: "[Descrição original do stakeholder]"

## Resumo

[Parágrafo de 2-3 linhas. O QUE é, POR QUE existe, QUEM usa.]

## Contexto e Motivação

[Qual problema resolve? Que dados já existem no sistema que alimentam essa feature?
Links para entidades relevantes, telas existentes, specs anteriores, etc.]

### Dados existentes que alimentam esta feature
- `tabela_x` — [descrição e campos relevantes]
- `tabela_y` — [descrição e relacionamentos]

### Telas existentes relacionadas
- `NomeDoComponente` (`/rota`) — [descrição]

## Personas Impactadas

| Persona | O que vê | O que faz |
|---|---|---|
| [Persona 1] | [descrever] | [descrever] |
| [Persona 2] | [descrever] | [descrever] |

---

## User Stories

<!--
  IMPORTANTE: User stories devem ser PRIORIZADAS como jornadas de usuário
  ordenadas por importância. Cada story deve ser INDEPENDENTEMENTE TESTÁVEL —
  se você implementar apenas UMA delas, deve ter um MVP viável.

  Atribua prioridades (P1, P2, P3) a cada story.
-->

### US-001: [Título da história] (Prioridade: P1)

**Como** [persona], **quero** [ação], **para que** [benefício].

**Por que esta prioridade**: [Explicar o valor e por que tem este nível de prioridade]

**Teste independente**: [Como verificar que esta story funciona sozinha]

#### Cenário Principal (Passo a Passo)

1. Usuário acessa [tela/rota]
2. Clica em [botão/ação]
3. Sistema exibe [modal/formulário/tela]
4. Usuário preenche [campos — detalhados abaixo]
5. Clica em [salvar/confirmar]
6. Sistema [o que acontece: toast, redirect, refresh de lista, etc.]

#### Campos do Formulário/Modal

| Campo | Tipo de Componente | Obrigatório | Fonte de Dados | Comportamento |
|---|---|---|---|---|
| Nome | `Input text` | ✅ | Manual | Max 255 chars |
| Responsável | `Select` com busca | ✅ | `GET /api/users` | Mostra `name`, envia `user_id` |
| Pipeline | `Select` | ❌ | `GET /api/pipelines` | Opção "Todos" como default |
| Período | `Select` | ✅ | Enum fixo | Ao mudar, auto-preenche datas |
| Método | `Select` | ✅ | Enum fixo | Ao mudar, campos condicionais mudam |
| Config OTE | `Grupo de inputs` | Condicional | — | Só aparece se método = "OTE" |

> **REGRA CRÍTICA**: Se existe dado no banco que pode popular um campo,
> o componente DEVE ser `Select` (ou `Combobox` com busca), NUNCA `Input text`.
> O usuário NUNCA deve digitar UUIDs, IDs, ou dados que o sistema já conhece.

#### Campos Condicionais

[Descrever quais campos aparecem/desaparecem baseado em seleções anteriores]

| Condição | Campos que aparecem |
|---|---|
| `tipo = "x"` | `campo_a`, `campo_b` |
| `tipo = "y"` | Tabela editável de faixas + toggle |

#### Cenários de Aceite (Given/When/Then)

1. **Dado** [estado inicial], **Quando** [ação], **Então** [resultado esperado]
2. **Dado** [estado inicial], **Quando** [ação], **Então** [resultado esperado]

#### Checklist de Aceite

- [ ] Modal/Tela abre sem erros no console
- [ ] Todos os selects carregam dados reais do sistema
- [ ] Campos condicionais respondem à seleção
- [ ] Validação no submit com feedback visual (toast i18n)
- [ ] Edição pré-popula todos os campos corretamente
- [ ] Exclusão pede confirmação antes de executar
- [ ] Visual segue o padrão do design system AllDone

#### Tratamento de Erros

| Erro | Comportamento esperado |
|---|---|
| Campo obrigatório vazio | Toast error com mensagem i18n |
| Resposta 400 do backend | Toast com `error.response.data.error` |
| Resposta 500 do backend | Toast genérico "Erro interno" |
| Rede indisponível | Toast "Sem conexão" |

---

### US-002: [Próxima história] (Prioridade: P2)

[Repetir o mesmo formato...]

---

### Casos Extremos (Edge Cases)

- O que acontece quando [condição limite]?
- Como o sistema reage a [cenário de erro]?
- E se [dependência externa] estiver indisponível?
- E se a lista estiver vazia (empty state)?
- E se o usuário não tiver permissão?

---

## Requisitos Funcionais

<!--
  Requisitos devem ser technology-agnostic e mensuráveis.
  Use [NEEDS CLARIFICATION] para requisitos ambíguos.
-->

- **FR-001**: O sistema DEVE [capacidade específica]
- **FR-002**: O sistema DEVE [capacidade específica]
- **FR-003**: O usuário DEVE ser capaz de [interação chave]

*Exemplo de requisito que precisa esclarecimento:*

- **FR-004**: O sistema DEVE autenticar via [NEEDS CLARIFICATION: método não especificado — email/senha, SSO, OAuth?]

## Entidades Envolvidas

<!--
  Descrever entidades do ponto de vista funcional (sem detalhes de implementação).
  Se a feature cria novas entidades ou modifica existentes, documentar aqui.
  Detalhes de schema SQL, migrations e índices vão no plan.md.
-->

### Entidades Novas

- **[Entidade 1]**: [O que representa, atributos principais, sem detalhes de implementação]
- **[Entidade 2]**: [O que representa, relacionamentos com outras entidades]

### Entidades Modificadas

| Entidade | Alteração | Motivo |
|---|---|---|
| [Entidade existente] | [O que muda] | [Por que precisa mudar] |

---

## Critérios de Sucesso

<!--
  Devem ser mensuráveis e technology-agnostic.
-->

- **SC-001**: [Métrica mensurável, ex: "Usuário completa o fluxo em menos de 3 cliques"]
- **SC-002**: [Métrica de performance, ex: "Sistema responde em menos de 500ms"]
- **SC-003**: [Métrica de negócio, ex: "Elimina necessidade de planilha externa"]

---

## Interface (Wireframes de Interação)

<!--
  Wireframes ASCII ou descrição detalhada da interação.
  Foco no COMPORTAMENTO, não na implementação visual exata.
-->

### Tela Principal

```
┌──────────────────────────────────────────┐
│ [← Voltar]  │  Título da Tela            │
├──────────────────────────────────────────┤
│ [Tab 1] [Tab 2] [Tab 3]                 │
├──────────────────────────────────────────┤
│                              [+ Novo]    │
│ ┌──────────────────────────────────────┐ │
│ │ 🟢 Item 1  Badge  Badge  [✏️] [🗑️] │ │
│ │   detalhe · detalhe · detalhe       │ │
│ │ 🟢 Item 2  Badge         [✏️] [🗑️] │ │
│ │   detalhe · detalhe                 │ │
│ └──────────────────────────────────────┘ │
│            [Empty state message]         │
└──────────────────────────────────────────┘
```

### Padrão Visual do Sistema AllDone (Referência)

| Elemento | Classe/Tamanho | Referência |
|---|---|---|
| `DialogContent` | `p-0`, `sm:max-w-[500px]`, `max-h-[90vh] flex flex-col` | `dialog.tsx` |
| `DialogHeader` | `px-4 pt-4 pb-3 border-b shrink-0` | Built-in |
| Body content | `flex-1 overflow-y-auto px-4 py-3` | `ProductFormModal.tsx` |
| `DialogFooter` | `px-4 py-3 border-t shrink-0` | Built-in |
| Input | `h-8 text-xs` | Sistema |
| SelectTrigger | `h-8 text-xs` | Sistema |
| Button (ação) | `size="sm" className="h-8 text-xs"` | Sistema |
| Button (back) | `variant="ghost" size="sm" className="h-8 w-8 p-0"` | `CRMSettings.tsx` |
| Page wrapper | `p-4 lg:p-6` > `max-w-7xl mx-auto space-y-4` | Sistema |
| List item | `p-3 border rounded-lg hover:bg-accent/50 transition-colors` | Sistema |
| Icon container | `h-9 w-9 rounded-full bg-primary/10` | Sistema |
| Action button | `variant="ghost" size="icon" className="h-7 w-7"` | Sistema |
| Badge | `text-[10px]` | Sistema |
| Sub-text | `text-[10px] text-muted-foreground` | Sistema |

---

## Internacionalização (i18n)

Todas as strings visíveis DEVEM usar `t("namespace.key")`.
Novas keys devem ser adicionadas em `apps/frontend/src/i18n/locales/pt-BR.json`.

### Keys Necessárias
```json
{
  "feature": {
    "pageTitle": "...",
    "subtitle": "...",
    "tabs": { ... },
    "modal": {
      "titleNew": "...",
      "titleEdit": "...",
      "fields": { ... },
      "validation": { ... },
      "success": { ... },
      "error": { ... }
    }
  }
}
```

---

## Clarifications

<!--
  Esta seção é preenchida durante a fase CLARIFY.
  A LLM DEVE fazer perguntas ao stakeholder para cada item marcado
  com [NEEDS CLARIFICATION] antes de iniciar o planejamento.
-->

> Toda ambiguidade deve ser resolvida aqui ANTES da fase PLAN.

| # | Pergunta | Resposta | Status |
|---|---|---|---|
| 1 | [Pergunta sobre ambiguidade] | [Resposta do stakeholder] | ✅ Resolvido |
| 2 | [Outra pergunta] | — | ❓ Pendente |

---

## Review & Acceptance Checklist

### Completude da Spec
- [ ] Todas as user stories têm cenários de aceite (Given/When/Then)
- [ ] Todas as user stories têm prioridade atribuída (P1, P2, P3)
- [ ] Cada user story é independentemente testável
- [ ] Todos os campos de formulário estão documentados com tipo de componente
- [ ] Todos os selects têm sua fonte de dados definida (API endpoint)
- [ ] Campos condicionais estão mapeados
- [ ] Requisitos funcionais listados com IDs (FR-XXX)
- [ ] Entidades envolvidas documentadas
- [ ] Critérios de sucesso mensuráveis definidos
- [ ] Casos extremos (edge cases) documentados
- [ ] Tratamento de erros documentado
- [ ] Nenhum `[NEEDS CLARIFICATION]` pendente
- [ ] Wireframes de interação incluídos

### Qualidade UX
- [ ] Nenhum campo pede ao usuário para digitar dados que o sistema já possui
- [ ] Todos os IDs são resolvidos para nomes legíveis na listagem
- [ ] Datas possuem auto-preenchimento quando contexto permite
- [ ] Campos condicionais respondem dinamicamente
- [ ] Empty states definidos para cada listagem
- [ ] Padrão visual referenciado (tamanhos, classes CSS)
````

---

## Template: plan.md

> Após a spec aprovada, o plano técnico deve ser salvo em:
> `docs/specs/NNN-feature-name/plan.md`

````markdown
# Plano Técnico: [Nome da Feature]

**Spec**: `NNN-feature-name`
**Data**: [DATA]
**Status**: Draft | Approved

## Resumo

[Extrair da spec: requisito principal + abordagem técnica]

## Pré-Requisitos

- [ ] Spec aprovada sem `[NEEDS CLARIFICATION]` pendente
- [ ] Entidades validadas
- [ ] Critérios de sucesso aceitos

## Contexto Técnico

**Stack**: TypeScript + Bun (runtime), Hono (backend), React + Vite (frontend)
**ORM**: Drizzle + PostgreSQL
**Auth**: Better Auth com GBAC (Group-Based Access Control)
**UI**: Shadcn/ui + Radix + Lucide icons
**i18n**: react-i18next (`pt-BR.json`)
**Testing**: Manual + curl (Vitest se disponível)
**Restrições**: [ex: < 500ms p95, offline-capable, ou N/A]

## Arquitetura

[Diagrama de componentes afetados — pode ser ASCII, Mermaid, ou descrição textual]

## Estrutura de Código

```
apps/
├── backend/src/
│   ├── routes/[feature].routes.ts      ← Endpoints + validação Zod
│   └── services/[feature].service.ts   ← Lógica de negócio
├── frontend/src/
│   ├── lib/api/[feature].ts            ← API client
│   ├── components/[feature]/           ← Componentes reutilizáveis
│   ├── pages/[Feature]Page.tsx         ← Tela principal
│   └── i18n/locales/pt-BR.json        ← Traduções
└── ...
```

## Modelo de Dados

### Tabelas Novas

| Tabela | Campos PK | Campos obrigatórios | FK | Índices |
|---|---|---|---|---|
| `nome_tabela` | `id` UUID | `tenant_id`, `name` | `tenant_id → tenants` | `(tenant_id, name)` |

### Tabelas Modificadas

| Tabela | Alteração | Motivo |
|---|---|---|
| `tabela_existente` | [O que muda] | [Por que] |

### Migration

```sql
-- Descrever SQL da migration
```

## API (Contratos)

### Endpoints Novos

| Método | Rota | Body | Response | Notas |
|---|---|---|---|---|
| `GET` | `/api/resource` | — | `{ items: T[] }` | Filtros: `?status=active` |
| `POST` | `/api/resource` | `{ field1, field2 }` | `{ item: T }` | 201 Created |
| `PUT` | `/api/resource/:id` | Partial body | `{ item: T }` | |
| `DELETE` | `/api/resource/:id` | — | `{ success: true }` | Soft delete |

### Endpoints Modificados

| Método | Rota | Alteração |
|---|---|---|
| `GET` | `/api/existing` | Adicionar filtro X |

### Validação (Zod Schemas)

```typescript
const resourceSchema = z.object({
    name: z.string().min(1).max(255),
    type: z.enum(["type_a", "type_b"]),
    config: z.record(z.unknown()).default({}),
});
```

## Ordem de Implementação

1. Data layer (migrations, schemas)
2. Backend (services → routes → validação)
3. Frontend (API client → componentes → telas)
4. i18n (keys)
5. Verificação final

## Componentes a Criar/Modificar

| Componente | Arquivo | Tipo | Descrição |
|---|---|---|---|
| `ComponentName` | `path/to/file.ext` | Novo | [Descrição] |
| `ExistingComponent` | `path/to/file.ext` | Modificar | [O que muda] |

## Riscos e Decisões

| # | Decisão | Alternativas | Justificativa |
|---|---|---|---|
| 1 | [Decisão tomada] | [Alternativa A, B] | [Por que escolheu essa] |

## Tracking de Complexidade

> Preencher APENAS se houver violações de princípios que precisem ser justificadas.

| Violação | Por que é necessária | Alternativa rejeitada porque |
|---|---|---|
| [ex: dependência adicional] | [necessidade atual] | [por que alternativa é insuficiente] |
````

---

## Template: tasks.md

> Após o plano aprovado, as tasks devem ser salvas em:
> `docs/specs/NNN-feature-name/tasks.md`

````markdown
# Tasks: [Nome da Feature]

**Input**: Documentos em `docs/specs/NNN-feature-name/`
**Pré-requisitos**: plan.md (obrigatório), spec.md (obrigatório para user stories)

## Formato

```
[ID] [P?] [Story] Descrição
```

- **[P]**: Pode rodar em paralelo (arquivos diferentes, sem dependências)
- **[Story]**: A qual user story pertence (ex: US1, US2, US3)
- Incluir caminhos exatos de arquivo nas descrições

## Regras

- 1 task = 1 entregável testável
- Cada task tem seu DOD (Definition of Done)
- Tasks são executadas em ordem (exceto as marcadas com [P] = paralelo)
- Não avançar para próxima task se a atual não atingiu o DOD
- Cada user story deve ser implementável e testável de forma independente

---

## Phase 1: Data Layer (Infraestrutura)

**Propósito**: Migrations e schemas que suportam toda a feature

- [ ] T001 [Migration] Criar/modificar tabelas conforme plan.md
  - **Arquivo**: `migrations/NNN_feature.sql`
  - **DOD**:
    - [ ] Migration roda sem erros
    - [ ] Tabelas criadas com tenant_id, FK, índices
    - [ ] `create_tables.sql` atualizado

---

## Phase 2: Backend

**Propósito**: API funcional com validação

- [ ] T002 [P] [US1] Implementar service em `services/feature.service.ts`
  - **DOD**:
    - [ ] Funções CRUD implementadas
    - [ ] Tenant isolation garantido
- [ ] T003 [US1] Implementar routes em `routes/feature.routes.ts`
  - **DOD**:
    - [ ] Endpoints CRUD funcionais
    - [ ] Validação Zod em todos os inputs
    - [ ] Respostas padronizadas
    - [ ] Testes manuais com curl passam
- [ ] T004 [P] [US1] Implementar controle de acesso/permissões
  - **DOD**:
    - [ ] GBAC aplicado nos endpoints
    - [ ] 403 retornado sem permissão

**Checkpoint**: Backend funcional — API responde corretamente

---

## Phase 3: Frontend — User Story 1 (P1) 🎯 MVP

**Objetivo**: [Breve descrição do que esta story entrega]
**Teste independente**: [Como verificar que funciona sozinha]

- [ ] T005 [P] [US1] API client em `lib/api/feature.ts`
  - **DOD**:
    - [ ] Todas as chamadas implementadas
    - [ ] Tipos corretos
- [ ] T006 [US1] Componente principal em `pages/FeaturePage.tsx`
  - **DOD**:
    - [ ] Tela renderiza sem erros no console
    - [ ] Selects carregam dados reais do sistema
    - [ ] Campos condicionais respondem à seleção
    - [ ] Validação no submit com toast i18n
    - [ ] Edição pré-popula todos os campos
    - [ ] Visual segue padrão AllDone (p-0, px-4 py-3, h-8 text-xs)
- [ ] T007 [US1] Navegação + rota + i18n
  - **DOD**:
    - [ ] Rota adicionada no router
    - [ ] Sidebar atualizada (se aplicável)
    - [ ] Keys de i18n adicionadas

**Checkpoint**: US-001 (MVP) totalmente funcional e testável

---

## Phase 4: Frontend — User Story 2 (P2)

**Objetivo**: [Breve descrição]
**Teste independente**: [Como verificar]

- [ ] T008 [US2] [Tarefa específica] em `path/to/file`
  - **DOD**: [...]

**Checkpoint**: US-002 funcional e testável independentemente

---

## Phase N: Polish & Verificação

**Propósito**: Melhorias que afetam múltiplas user stories

- [ ] TXXX Verificação final: TypeScript build sem erros (`npm run build`)
- [ ] TXXX [P] i18n: todas as strings traduzidas
- [ ] TXXX Code cleanup e otimizações

---

## Dependências & Ordem de Execução

### Entre Fases

- **Data Layer (Phase 1)**: Sem dependências — começa imediatamente
- **Backend (Phase 2)**: Depende de Data Layer
- **Frontend (Phase 3+)**: Depende de Backend
  - User Stories podem prosseguir sequencialmente por prioridade (P1 → P2 → P3)
- **Polish (Phase Final)**: Depende de todas as user stories desejadas

### Dentro de Cada User Story

- API client antes de componentes
- Componentes antes de telas
- Implementação core antes de integração
- Story completa antes de avançar para a próxima

### Estratégia MVP First

1. Completar Phase 1: Data Layer
2. Completar Phase 2: Backend
3. Completar Phase 3: User Story 1 (P1 = MVP)
4. **PARAR e VALIDAR**: Testar US-001 de forma independente
5. Se ok, avançar para US-002
6. Cada story adiciona valor sem quebrar as anteriores
````

---

## Instruções para a LLM

### Ao receber um pedido de nova funcionalidade:

1. **NÃO comece a codar imediatamente**
2. **Crie a spec** seguindo o template `spec.md` acima
3. **Faça perguntas** (fase CLARIFY) — exemplos de boas perguntas:
   - "Quem são os usuários dessa funcionalidade? Que tipo de acesso cada um tem?"
   - "Quando o usuário seleciona [campo X], quais campos novos aparecem/desaparecem?"
   - "Esse campo é obrigatório ou opcional? Se opcional, qual é o comportamento default?"
   - "Qual é o comportamento esperado quando [condição de erro]?"
   - "O select de [entidade] deve mostrar todos os registros ou filtrado por [critério]?"
   - "Precisa de barra de progresso na listagem ou só no detalhe?"
   - "O gerente pode fazer [ação] em lote ou apenas individualmente?"
4. **Só depois de todas as clarifications resolvidas**, crie o plano técnico (`plan.md`)
5. **Só depois do plano aprovado**, decomponha em tasks (`tasks.md`)
6. **Execute uma task por vez**, verificando o DOD de cada uma

### Regras de Qualidade Invioláveis

1. **Zero campos com UUID manual** — se o dado existe no sistema, usar `Select`/`Combobox`
2. **Zero modais sem padding** — seguir padrão `p-0` + `px-4 py-3` body
3. **Zero strings hardcoded** — tudo com `t("key")`
4. **Zero funcionalidade sem botão de criação** — se lista existe, botão "+ Novo" existe
5. **Zero campos condicionais sem mapeamento** — se o tipo muda, os campos mudam
6. **Zero formulários sem validação** — Zod no backend, feedback visual no frontend
7. **Zero listagens sem resolver IDs → nomes** — mostrar nome humano, não UUID
8. **Zero ambiguidade não resolvida** — todo `[NEEDS CLARIFICATION]` respondido antes de planejar
9. **Zero user stories sem cenários de aceite** — Given/When/Then para cada story
10. **Zero implementação sem prioridade** — P1 primeiro, sempre

### Sinais de que a Spec Precisa de Mais Trabalho

| Sinal | Ação |
|---|---|
| User story descreve implementação em vez de comportamento | Reescrever focando no "O QUÊ" |
| Cenário de aceite menciona tecnologia específica | Remover referências técnicas, focar no resultado |
| Requisito funcional é vago ("sistema deve ser rápido") | Quantificar ("resposta < 500ms") |
| Nenhum edge case documentado | Adicionar pelo menos 3 cenários de erro/limite |
| Entidades novas sem relacionamentos descritos | Completar descrição funcional |
| User stories não são independentemente testáveis | Reorganizar para que cada uma gere um MVP parcial |
| Campo usa Input text para dado que o sistema já possui | Trocar para Select/Combobox com fonte de dados |
| Select sem fonte de dados definida | Adicionar API endpoint ou enum fixo |
| Nenhum empty state descrito | Documentar o que aparecer quando a lista está vazia |
| Formulário sem tabela de campos | Adicionar tabela com Tipo de Componente, Fonte de Dados |

