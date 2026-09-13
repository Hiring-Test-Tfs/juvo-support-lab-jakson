# Pergunta reversa e declaração de IA

**Navegação:** [Índice](../INDICE.md) · [Entrega](../ENTREGA.md) · [Modelo de trilha](./MODELO-TRILHA.md)

Este arquivo é parte da entrega. Preencha **depois** de `TRIAGEM.md` e `TRILHAS.md` - use o que você sentiu ao resolver os 12 tickets.

---

## Por que pedimos isso?

No suporte sênior, nem todo ticket vem completo. Parte do trabalho é perceber **o que falta**, **o que está ambíguo** e **qual pergunta fazer** (ao CX, ao produto, à engenharia ou à operação) antes de escalar ou fechar.

A **pergunta reversa** inverte o papel: em vez de só responder aos casos que demos, você mostra **como pensa quando o cenário não fecha** - o mesmo hábito de quem pede contexto, confirma política interna ou questiona dado inconsistente antes de agir.

**Não é prova oral.** Não precisa ser perfeito. Queremos ver **critério** e **comunicação**.

---

## 1. Pergunta reversa - como preencher

Escolha **um único** ponto do teste (um ticket, o [export CSV](../data/sistema-interno-export.csv), o [glossário](./GLOSSARIO.md), etc.) e responda **as três partes** abaixo.

### a) Contexto

Qual caso ou lacuna você escolheu? (1-2 frases)

> **Exemplo:** No [IT-2001](../tickets/IT-2001.md), o export mostra `ANTIFRAUD_HOLD`, mas não há indicação de quem libera o hold nem SLA.

### b) Sua pergunta ao time

O que você **perguntaria** à Juvo (suporte, produto, engenharia ou operação) para destravar ou decidir com segurança?

> **Exemplo:** *“Quem analisa holds de antifraude e qual o canal/SLA para o suporte acompanhar a liberação por CCB?”*

### c) Por que isso importa

Como a resposta **mudaria sua trilha** (passos 5-7 em [MODELO-TRILHA](./MODELO-TRILHA.md))? O que você faria diferente?

> **Exemplo:** *“Se o suporte pode abrir chamado na fila de risco com evidência, eu escalaria já. Se há painel de acompanhamento, manteria EM ANÁLISE e monitoraria.”*

---

### O que evitar

- Pergunta genérica (*“como funciona o sistema interno?”*) - seja específico ao material que você leu.
- Copiar o texto do ticket sem acrescentar dúvida real.
- Listar vários temas - **um** ponto bem argumentado vale mais.

### Ideias válidas (se ajudar)

| Tema | Exemplo de pergunta |
|------|---------------------|
| Política / SLA | No [IT-2012](../tickets/IT-2012.md): qual prazo oficial de desembolso pós-assinatura para orientar o CX? |
| PIX duplicado | No [IT-2003](../tickets/IT-2003.md): há runbook de conciliação quando o Banco retorna dois `payment_id` para a mesma parcela? |
| Mesmo cliente, CCBs diferentes | [IT-2008](../tickets/IT-2008.md) vs [IT-2011](../tickets/IT-2011.md): há regra de prioridade quando o CX abre dois desembolsos? |
| Valor divergente | No [IT-2009](../tickets/IT-2009.md): quem decide baixa quando o comprovante do CX ≠ `payment_amount_brl` do extrato? |

---

### Sua resposta (preencha aqui)

**a) Contexto:**

> No IT-2008 e no IT-2011, o mesmo CPF aparece em duas CCBs: a 90008001 está recusada/cancelada por `RECUSA_CREDITO`, enquanto a 90008002 está assinada e aguardando desembolso. O material não informa a regra de prioridade ou o SLA quando o CX abre tickets para contratos diferentes do mesmo cliente.

**b) Sua pergunta ao time:**

> “Qual é o SLA e a regra operacional para priorizar a CCB 90008002, assinada e aguardando desembolso, quando existe outra CCB do mesmo CPF (90008001) recusada/cancelada? Há algum bloqueio de cliente ou devo tratar cada CCB de forma independente?”

**c) Por que isso importa / o que mudaria na trilha:**

> Se as CCBs forem independentes, mantenho o IT-2008 solucionado por orientação e acompanho e escalo apenas a fila da 90008002 conforme o SLA. Se houver bloqueio por cliente ou regra de prioridade entre propostas, registro a dependência entre os tickets e escalo para crédito/operação antes de agir na segunda CCB.

---

## 2. Declaração de uso de IA

**Por quê:** IA é permitida neste teste. Queremos transparência sobre **onde** você usou e **o que** permaneceu seu raciocínio (triagem, hipóteses, escalações).

Preencha com honestidade - usar IA para organizar texto ou revisar redação **não penaliza**; omitir uso quando houve, sim.

| Campo | Resposta |
|-------|----------|
| Usou IA neste teste? | Sim |
| Ferramenta(s) | GitHub Copilot |
| Para quê? | Organizar a leitura dos documentos, cruzar os tickets com o CSV e revisar a redação e estrutura das trilhas e as anotações que fiz para cada ticket. |
| O que **não** delegou à IA? | A validação das evidências que fiz uma a uma com base no csv, a ordem da triagem de acordo com o que julguei mais critico ou menos critico, as hipóteses por ticket e as decisões de resolver, quando orientar ou escalar. |

**Comentário opcional** (1-3 linhas):

> A decisão final foi baseada nos campos do export e nas regras do fluxo; a IA foi usada como apoio de organização para que tudo fique bem estruturado e legivel.
