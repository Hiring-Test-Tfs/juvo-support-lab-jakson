# Fluxo IT-Support (resumo)

**Navegação:** [Índice](../INDICE.md) · [Fila](../FILA.md) · [Glossário](./GLOSSARIO.md) · [Modelo de trilha](./MODELO-TRILHA.md) · [Entrega](../ENTREGA.md)

Como o time de suporte costuma trabalhar tickets do projeto **IT** no Jira.

## 1. Chegada do ticket

- CX abre pelo **IT-Support** (Slack ou portal).
- O ticket cai no Jira com tipo **Support**, prioridade geralmente **Crítica**.
- Campos padrão: Problema, Nome, CPF, CCB, Solicitado por, Descrição.

## 2. Triagem

1. Ler a descrição e identificar a **categoria** (desembolso, pagamento, onboarding, etc.).
2. Buscar o cliente no **sistema interno** pelo CPF ou CCB.
3. Comparar o que o CX relatou com o **status real** no sistema.
4. Decidir: resolve no suporte, orienta o CX, ou escala para engenharia / risco / cobrança.

## 3. Resposta no ticket

Padrão observado nos tickets reais:

- Comentário curto com o que foi feito: *"Boleto regenerado"*, *"Termo reenviado"*, *"Caso escalado para antifraude"*.
- Mencionar quem abriu: `@agente.cx01` (handles fictícios neste teste).
- Anexar print do sistema interno quando fizer sentido (no teste, descreva o que anexaria).
- Mudar status para **EM ANÁLISE** enquanto investiga, **SOLUCIONADO** quando concluir, **ESCALADO** quando passar adiante.

## 4. Quando escalar

Escale quando:

- Status no sistema interno **contradiz** o relato e não há ação manual segura
- Hold de **antifraude** / risco sem botão operacional de liberação
- **Dois pagamentos** ou valor do comprovante **≠** valor no sistema
- Bug de onboarding/KYC que exige time de produto ou fornecedor de identidade

Não escale quando:

- Falta só **regenerar boleto** vencido ou reenviar documento
- Orientar CX sobre **prazo** sem erro técnico
- Contrato **recusado/cancelado** com motivo claro - orientar, não reprocessar PIX

## 5. Volume típico da fila

1. **Desembolso ao cliente** - contrato assinado, cliente sem PIX  
2. **Onboarding** - erro de documento/KYC  
3. **Pagamento** - PIX pago, parcela não baixada (ou divergência)  
4. **Cobrança** - boleto vencido / documento errado  
5. **Renegociação** - valor ou regra inconsistente  
6. **Comunicação / LGPD** - opt-out SMS  

Este teste traz **12 tickets** cobrindo esse mix, incluindo **mesmo CPF com CCBs diferentes**, hold de antifraude e dúvida de CX que pode ser rebaixada.
