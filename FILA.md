# Fila IT-Support - 06/08/2026 (manhã)

**Navegação:** [Índice](./INDICE.md) · [Export sistema interno](./data/sistema-interno-export.csv) · [Modelo de trilha](./docs/MODELO-TRILHA.md) · [Entrega](./ENTREGA.md)

Snapshot **fictício** da fila **NOVO** ao assumir o turno (~09:45).

> Todos os nomes, CPFs, CCBs e e-mails deste teste são **inventados**. Não correspondem a clientes reais.

Todos chegaram com prioridade **Crítica** no Jira (padrão do formulário). **Você** define a ordem real de atendimento.

## Resumo da fila

| # | Ticket | Problema | CPF (fictício) | Aberto por | Hora |
|---|--------|----------|----------------|------------|------|
| 1 | IT-2001 | Desembolso ao cliente | 902.111.111-01 | @agente.cx01 | 09:08 |
| 2 | IT-2002 | Onboarding site | 902.222.222-02 | @agente.cx02 | 09:15 |
| 3 | IT-2003 | Pagamento do cliente | 902.333.333-03 | @agente.cx03 | 09:22 |
| 4 | IT-2004 | Erro parcela cobrança (sistema interno) | 902.444.444-04 | @agente.cx04 | 09:29 |
| 5 | IT-2005 | Erro negociação (Juvo Negocia) | 902.555.555-05 | @agente.cx03 | 09:36 |
| 6 | IT-2006 | Parar SMS / comunicação | 902.666.666-06 | @agente.cx05 | 09:43 |
| 7 | IT-2007 | Termo de quitação | 902.777.777-07 | @agente.cx06 | 09:50 |
| 8 | IT-2008 | Desembolso ao cliente | 902.888.888-08 | @agente.cx07 | 09:57 |
| 9 | IT-2009 | Pagamento do cliente | 902.999.999-09 | @agente.cx08 | 10:04 |
| 10 | IT-2010 | Pagamento do cliente | 902.101.010-10 | @agente.cx09 | 10:11 |
| 11 | IT-2011 | Desembolso ao cliente | 902.888.888-08 | @agente.cx07 | 10:18 |
| 12 | IT-2012 | Dúvida - prazo de desembolso | 902.121.212-12 | @agente.cx10 | 10:25 |

## Abrir ticket

1. [IT-2001 - Desembolso (antifraude)](./tickets/IT-2001.md)
2. [IT-2002 - Onboarding KYC](./tickets/IT-2002.md)
3. [IT-2003 - Pagamento (PIX duplicado)](./tickets/IT-2003.md)
4. [IT-2004 - Boleto vencido](./tickets/IT-2004.md)
5. [IT-2005 - Renegociação (valor)](./tickets/IT-2005.md)
6. [IT-2006 - Parar SMS](./tickets/IT-2006.md)
7. [IT-2007 - Termo de quitação](./tickets/IT-2007.md)
8. [IT-2008 - Desembolso (CCB recusada)](./tickets/IT-2008.md)
9. [IT-2009 - Pagamento (valor divergente)](./tickets/IT-2009.md)
10. [IT-2010 - Pagamento (alega quitação)](./tickets/IT-2010.md)
11. [IT-2011 - Desembolso (2ª CCB)](./tickets/IT-2011.md)
12. [IT-2012 - Dúvida prazo](./tickets/IT-2012.md)

Cruze cada ticket com o [export do sistema interno](./data/sistema-interno-export.csv) (busque pelo CPF).

**Dica:** nem todo ticket precisa de escalação. Alguns são resolução rápida; outros têm dados inconsistentes no export. Tickets do **mesmo CPF** podem ser **CCBs diferentes**.
