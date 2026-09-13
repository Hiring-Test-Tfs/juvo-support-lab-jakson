# Triagem IT-Support - 06/08/2026

Ordem definida considerando risco financeiro, bloqueio do cliente, possibilidade de ação segura no suporte e necessidade de escalação. A prioridade `Crítica` recebida pelo formulário não diferencia os casos.

1. **[IT-2003](./tickets/IT-2003.md) - PIX duplicado**
   Há dois `payment_id` para a parcela 5, que permanece aberta. Existe risco de conciliação duplicada e cobrança indevida; escalar imediatamente para cobrança/financeiro e banco, sem baixar manualmente antes da confirmação.
2. **[IT-2009](./tickets/IT-2009.md) - Valor divergente**
   O comprovante informado é de R$ 450,00, enquanto o sistema registra R$ 403,06. A divergência impede uma baixa segura e exige conciliação com cobrança/financeiro.
3. **[IT-2001](./tickets/IT-2001.md) - Hold de antifraude**
   O contrato está assinado, mas o desembolso está retido em `ANTIFRAUD_HOLD`. Escalar para risco/antifraude com os dados da tentativa.
4. **[IT-2002](./tickets/IT-2002.md) - Erro de KYC**
   O cliente aprovado não consegue concluir documento/selfie e o export registra `KYC_DOC_EXPIRED`. É bloqueio de onboarding com provável dependência de produto/fornecedor de identidade.
5. **[IT-2011](./tickets/IT-2011.md) - Desembolso da segunda CCB**
   A CCB 90008002 está assinada e aguardando desembolso há 16 horas, sem erro informado. Investigar a fila e o prazo; relacionar com IT-2008, mas não misturar os contratos.
6. **[IT-2005](./tickets/IT-2005.md) - Valor de renegociação divergente**
   A proposta verbal de R$ 2.450,00 não coincide com os R$ 1.890,00 do sistema. Não alterar o valor; encaminhar para cobrança/negociação validar a proposta.
7. **[IT-2010](./tickets/IT-2010.md) - Parcela alegadamente paga**
   A parcela 7 continua aberta e não há comprovante. Solicitar evidência antes de qualquer baixa manual.
8. **[IT-2004](./tickets/IT-2004.md) - Boleto vencido**
   É uma correção operacional: inativar o documento vencido e gerar boleto válido de R$ 412,50.
9. **[IT-2007](./tickets/IT-2007.md) - Termo de quitação**
   O contrato está quitado e só há falha de entrega por `bounce`; corrigir o e-mail validado e reenviar o termo.
10. **[IT-2006](./tickets/IT-2006.md) - Opt-out de SMS**
   O pedido é apenas de comunicação comercial, não de exclusão LGPD. Aplicar o opt-out e orientar o CX.
11. **[IT-2008](./tickets/IT-2008.md) - CCB recusada/cancelada**
   A CCB 90008001 está `recusado`/`cancelado` por `RECUSA_CREDITO`; não se deve reprocessar o PIX. Orientar o CX com o motivo e registrar a relação com IT-2011.
12. **[IT-2012](./tickets/IT-2012.md) - Prazo de desembolso**
   Assinatura ocorreu há apenas 2 horas, a conta está validada e não há erro. É orientação de prazo, não liberação manual nem incidente crítico.

IT-2008 e IT-2011 são do mesmo CPF, porém têm CCBs distintas (`90008001` e `90008002`) e estados opostos. Serão investigados em conjunto para contexto, mas tratados separadamente e sem reprocessar a CCB recusada.