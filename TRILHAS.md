# Trilhas de troubleshooting

As consultas abaixo usam o export como primeira evidência. Em produção, eu faria a consulta read-only no sistema interno e usaria os fluxos operacionais autorizados para qualquer alteração.

## IT-2001 - Desembolso ao cliente (antifraude)

### Trilha de troubleshooting

1. **Onde consulto primeiro:** Sistema interno -> CPF `90211111101` e CCB `90001001` -> desembolso e fila de risco.
2. **O que busco:** Status do contrato, status do desembolso, erro da última tentativa e se a conta foi validada.
3. **O que encontrei:** CSV: `contract_status=assinado`, `disbursement_status=aguardando_desembolso`, `disbursement_error=ANTIFRAUD_HOLD`, `last_disbursement_attempt=2026-08-06T05:10:00Z`, `bank_validated=sim`, `credit_status=aprovado_alpha9`.
4. **Hipótese:** O desembolso está retido pelo sistema antifraude, não bloqueado por conta inválida.
5. **Retry / reprocesso:** N/A - somente repetir o PIX não remove o bloqueio preventivo e pode gerar tentativa indevida.
6. **Correção manual no sistema interno:** N/A - não liberar nem alterar o bloqueio sem decisão do time de risco.
7. **Escalação:** Risco/antifraude, com CPF, CCB, erro e horário da tentativa; anexaria print da tela do hold.
8. **Comunicação no Jira:** `@agente.cx01` Consultei a CCB 90001001: contrato assinado, conta validada e desembolso retido em `ANTIFRAUD_HOLD` desde 05:10. Escalei para antifraude e não reprocessarei o PIX sem liberação.
9. **Status final:** ESCALADO

## IT-2002 - Onboarding site (KYC)

### Trilha de troubleshooting

1. **Onde consulto primeiro:** Sistema interno -> CPF `90222222202` -> jornada de onboarding, logs do fornecedor de identidade se houver.
2. **O que busco:** Etapa atual, código da falha, tentativas, validade do documento e status de crédito.
3. **O que encontrei:** CSV: `onboarding_step=document_upload`, `onboarding_error=KYC_DOC_EXPIRED`, `credit_status=aprovado_alpha9`, `bank_validated=sim`, `lead_status=lead_ativo`.
4. **Hipótese:** O bloqueio é causado por documento expirado na etapa de upload, não pelo aparelho usado.
5. **Retry / reprocesso:** N/A - repetir no mesmo estado tende a reproduzir `KYC_DOC_EXPIRED`; primeiro é necessário renovar o documento/fluxo.
6. **Correção manual no sistema interno:** N/A - suporte não deve aprovar documento nem alterar o resultado do KYC manualmente.
7. **Escalação:** Produto/fornecedor de identidade, com CPF, etapa, erro e evidência das tentativas; pedir reset/reabertura somente conforme manual de operações.
8. **Comunicação no Jira:** `@agente.cx02` O cliente está aprovado no Alpha9, mas o onboarding parou em `document_upload` com `KYC_DOC_EXPIRED`. Escalei para produto/identidade para validar a reabertura do KYC; anexaria print e logs.
9. **Status final:** ESCALADO

## IT-2003 - Pagamento (PIX duplicado)

### Trilha de troubleshooting

1. **Onde consulto primeiro:** Sistema interno -> CPF `90233333303`, CCB `90003001` -> parcela 5, pagamentos e conciliação bancária.
2. **O que busco:** Estado da parcela, valor esperado, todos os `payment_id`, status e horários retornados pelo banco.
3. **O que encontrei:** CSV: `installment_ref=parcela_5`, `installment_status=aberta`, `installment_amount_brl=380.00`, `payment_id=banco_pay_r2_d01;banco_pay_r2_d02`, `payment_status=pago`, `payment_amount_brl=380.00`, `payment_received_at=2026-08-05T14:20:00Z`.
4. **Hipótese:** Há dois registros de pagamento de R$ 380,00 para a mesma parcela; a baixa automática não ocorreu por duplicidade.
5. **Retry / reprocesso:** N/A - não reprocessar nem gerar nova cobrança enquanto a duplicidade não for conciliada.
6. **Correção manual no sistema interno:** N/A - não baixar ou estornar manualmente antes de o banco confirmar qual pagamento deve quitar a parcela e como tratar o valor excedente.
7. **Escalação:** Cobrança/financeiro, com os dois `payment_id`, valor da parcela e comprovantes; solicitar conciliação/estorno ou crédito do segundo pagamento.
8. **Comunicação no Jira:** `@agente.cx03` Identifiquei a parcela 5 aberta e dois pagamentos feitos de R$ 380,00 (`banco_pay_r2_d01` e `banco_pay_r2_d02`). Escalei para conciliação com cobrança/banco; não farei baixa manual até confirmar o tratamento do duplicado.
9. **Status final:** ESCALADO

## IT-2004 - Boleto vencido

### Trilha de troubleshooting

1. **Onde consulto primeiro:** Sistema interno -> CPF `90244444404`, CCB `90004001` -> parcela 4 e documentos de cobrança.
2. **O que busco:** Parcela aberta, valor, documento ativo e estado/vencimento do boleto.
3. **O que encontrei:** CSV: `installment_ref=parcela_4`, `installment_status=aberta`, `installment_amount_brl=412.50`, `last_doc_generated=doc_r2_0401`, `last_doc_type=boleto_parcela`, `last_doc_status=vencida`.
4. **Hipótese:** O boleto ativo está vencido e precisa ser substituído por documento com vencimento atual.
5. **Retry / reprocesso:** N/A - não é falha de job; é documento vencido.
6. **Correção manual no sistema interno:** Inativar/cancelar `doc_r2_0401` e gerar novo boleto da parcela 4 no valor de R$ 412,50, conferindo que apenas o novo documento permaneça ativo.
7. **Escalação:** N/A - regeneração de boleto é ação operacional do suporte.
8. **Comunicação no Jira:** `@agente.cx04` Boleto vencido `doc_r2_0401` inativado e novo boleto da parcela 4 (R$ 412,50) gerado com vencimento atual. Anexaria print do documento novo.
9. **Status final:** SOLUCIONADO

## IT-2005 - Renegociação (valor)

### Trilha de troubleshooting

1. **Onde consulto primeiro:** Sistema interno/Juvo Negocia -> CPF `90255555505`, CCB `90005001` -> elegibilidade e proposta atual.
2. **O que busco:** Valor exibido, regra que o calculou, dias de atraso, parcelas aceleradas e bloqueios da renegociação.
3. **O que encontrei:** CSV: `renegotiation_eligible=sim`, `installment_ref=proposta_reneg`, `installment_status=aberta`, `installment_amount_brl=1890.00`, `days_past_due=95`, `accelerated_installments` sem valor e `renegotiation_block_reason` sem valor. O CX relata proposta verbal de R$ 2.450,00.
4. **Hipótese:** A proposta verbal não corresponde ao cálculo/regra vigente no Juvo Negocia; não há base para alterar o valor manualmente.
5. **Retry / reprocesso:** N/A - repetir a geração não resolve divergência de regra.
6. **Correção manual no sistema interno:** N/A - não editar o valor de R$ 1.890,00 nem formalizar R$ 2.450,00 sem aprovação da política responsável.
7. **Escalação:** Cobrança/negociação, para confirmar a oferta autorizada e investigar a origem dos R$ 2.450,00; anexaria print da proposta e o contexto do CX.
8. **Comunicação no Jira:** `@agente.cx03` A CCB 90005001 está elegível e com 95 dias de atraso, mas o sistema calcula R$ 1.890,00; a oferta verbal informada é R$ 2.450,00. Escalei para cobrança/negociação validar a regra antes de formalizar.
9. **Status final:** ESCALADO

## IT-2006 - Parar SMS / comunicação

### Trilha de troubleshooting

1. **Onde consulto primeiro:** Sistema interno -> CPF `90266666606` -> preferências de comunicação e cadastro do lead.
2. **O que busco:** Preferência atual de SMS, consentimento, status do lead e se há pedido de exclusão total.
3. **O que encontrei:** CSV: `onboarding_step=completed`, `credit_status=reprovado_alpha9`, `lead_status=lead_ativo`, `lgpd_delete_requested=nao`, `bank_validated=nao`.
4. **Hipótese:** É um opt-out de SMS comercial, sem solicitação de exclusão completa dos dados.
5. **Retry / reprocesso:** N/A - não há job ou transação a reprocessar.
6. **Correção manual no sistema interno:** Aplicar o opt-out de SMS na preferência de comunicação, preservando os demais registros e canais conforme a política interna.
7. **Escalação:** N/A - pedido de opt-out é tratável no suporte.
8. **Comunicação no Jira:** `@agente.cx05` Registrei o opt-out de SMS comercial para o CPF 902.666.666-06. O pedido não é de exclusão total de dados; anexaria print da preferência atualizada.
9. **Status final:** SOLUCIONADO

## IT-2007 - Termo de quitação

### Trilha de troubleshooting

1. **Onde consulto primeiro:** Sistema interno -> CPF `90277777707`, CCB `90007001` -> contrato quitado, documentos e histórico de e-mail.
2. **O que busco:** Status do contrato, termo gerado, endereço cadastrado e motivo do retorno do provedor.
3. **O que encontrei:** CSV: `contract_status=quitado`, `term_email_status=bounce`; não há outro documento pendente indicado no export.
4. **Hipótese:** O termo foi disponibilizado para envio, mas o e-mail cadastrado foi rejeitado pelo provedor.
5. **Retry / reprocesso:** N/A - reenviar para o mesmo endereço inválido repetiria o problema; primeiro validar o novo e-mail com o CX.
6. **Correção manual no sistema interno:** Atualizar o e-mail após validação do titular e reenviar o termo de quitação; conferir o novo status de entrega.
7. **Escalação:** N/A - correção cadastral e reenvio são ações operacionais do suporte.
8. **Comunicação no Jira:** `@agente.cx06` Contrato 90007001 está quitado, mas o envio do termo retornou `bounce`. Após validar/corrigir o e-mail, reenviei o termo e anexaria o novo status de entrega.
9. **Status final:** SOLUCIONADO

## IT-2008 - Desembolso (CCB recusada)

### Trilha de troubleshooting

1. **Onde consulto primeiro:** Sistema interno -> CPF `90288888808` e CCB informada `90008001` -> contrato e desembolso; também verifico se há outra CCB para o mesmo CPF.
2. **O que busco:** Status exato da CCB 90008001, motivo do cancelamento, tentativa de desembolso e possíveis contratos relacionados.
3. **O que encontrei:** CSV: na linha da CCB `90008001`, `contract_status=recusado`, `disbursement_status=cancelado`, `disbursement_error=RECUSA_CREDITO`, `last_disbursement_attempt=2026-08-05T16:00:00Z`, `bank_validated=sim`. O mesmo CPF também possui a CCB `90008002`, mas ela é outro contrato.
4. **Hipótese:** A CCB 90008001 não aguarda um PIX: crédito recusado.
5. **Retry / reprocesso:** N/A - não reprocessar PIX de contrato recusado/cancelado.
6. **Correção manual no sistema interno:** N/A - não reabrir contrato nem alterar o motivo de recusa; orientar sobre a decisão conforme a comunicação autorizada.
7. **Escalação:** N/A - o motivo é claro e o fluxo orienta informar, não reprocessar. Só escalar para crédito se o cliente contestar a decisão com evidência nova.
8. **Comunicação no Jira:** `@agente.cx07` A CCB 90008001 está `recusado`/`cancelado` com `RECUSA_CREDITO`; não há PIX para reprocessar.
9. **Status final:** SOLUCIONADO

## IT-2009 - Pagamento (valor divergente)

### Trilha de troubleshooting

1. **Onde consulto primeiro:** Sistema interno -> CPF `90299999909`, CCB `90009001` -> parcela 3.
2. **O que busco:** Valor da parcela, valor/ID do pagamento, data, comprovante anexado e correspondência do recebedor.
3. **O que encontrei:** CSV: `installment_ref=parcela_3`, `installment_status=aberta`, `installment_amount_brl=403.06`, `payment_id=banco_pay_r2_i01`, `payment_status=pago`, `payment_amount_brl=403.06`, `payment_received_at=2026-08-05T11:05:00Z`. O comprovante do ticket informa R$ 450,00.
4. **Hipótese:** O comprovante apresentado não corresponde ao pagamento registrado para a parcela 3 ou há pagamento excedente.
5. **Retry / reprocesso:** N/A - não reprocessar a baixa sem conciliar valor.
6. **Correção manual no sistema interno:** N/A - não baixar R$ 450,00 na parcela de R$ 403,06 nem alterar valor sem confirmação de cobrança/financeiro.
7. **Escalação:** Cobrança/financeiro e banco para conciliar o comprovante de R$ 450,00, o `payment_id` `banco_pay_r2_i01` e a parcela; anexaria o comprovante.
8. **Comunicação no Jira:** `@agente.cx08` A parcela 3 continua aberta e o export registra pagamento de R$ 403,06 (`banco_pay_r2_i01`), enquanto o comprovante anexado é de R$ 450,00. Escalei para conciliação e não farei baixa manual com valor divergente.
9. **Status final:** ESCALADO

## IT-2010 - Pagamento (alega quitação)

### Trilha de troubleshooting

1. **Onde consulto primeiro:** Sistema interno -> CPF `90210101010`, CCB `90010001` -> parcela 7, parcelas em atraso.
2. **O que busco:** Status e valor da parcela, pagamentos associados, data da alegada quitação e comprovante; solicitar o comprovante ao CX.
3. **O que encontrei:** CSV: `contract_status=ativo`, `installment_ref=parcela_7`, `installment_status=aberta`, `installment_amount_brl=390.00`, `payment_provider=banco`, `days_past_due=45`; não há `payment_id`, status ou valor de pagamento no registro.
4. **Hipótese:** Não há evidência no export de pagamento da parcela 7; a baixa não pode ser feita apenas pela alegação feita.
5. **Retry / reprocesso:** N/A - não existe pagamento identificado para reprocessar; pedir comprovante e dados da transação.
6. **Correção manual no sistema interno:** N/A - não baixar a parcela sem localizar e validar o pagamento no banco.
7. **Escalação:** N/A por enquanto; pedir ao CX o comprovante. Escalar para cobrança/banco depois de recebê-lo, se houver divergência ou pagamento não conciliado.
8. **Comunicação no Jira:** `@agente.cx09` A CCB 90010001 tem a parcela 7 aberta, no valor de R$ 390,00, e 45 dias de atraso, sem pagamento identificado no export. Preciso do comprovante/data/ID para consultar o banco antes da baixa.
9. **Status final:** EM ANÁLISE

## IT-2011 - Desembolso (segunda CCB)

### Trilha de troubleshooting

1. **Onde consulto primeiro:** Sistema interno -> CPF `90288888808` e CCB `90008002` -> contrato, fila de desembolso e histórico; comparo separadamente com a CCB 90008001 do IT-2008.
2. **O que busco:** Status da CCB 90008002, horário da assinatura, erro, validação bancária.
3. **O que encontrei:** CSV: `ccb=90008002`, `contract_status=assinado`, `disbursement_status=aguardando_desembolso`, `disbursement_error` vazio, `last_disbursement_attempt=2026-08-05T20:30:00Z`, `bank_validated=sim`, `hours_since_signature=16`. A CCB 90008001 é outra linha e está recusada/cancelada.
4. **Hipótese:** A segunda CCB está em espera normal ou com atraso de fila, sem erro bancário; não é o mesmo incidente da CCB recusada.
5. **Retry / reprocesso:** N/A neste primeiro momento - não há erro que justifique novo PIX; verificar fila/SLA e só reprocessar se o manual de operações autorizar após identificar falha.
6. **Correção manual no sistema interno:** N/A - não alterar o contrato nem confundir a CCB 90008002 com a 90008001.
7. **Escalação:** Operação de desembolso/bancarizador se o prazo operacional já tiver vencido; enviar CCB, horário, estado e confirmação bancária. Manter em análise enquanto se confirma o SLA.
8. **Comunicação no Jira:** `@agente.cx07` Separei as CCBs: 90008001 está recusada/cancelada, enquanto 90008002 está assinada, com conta validada e `aguardando_desembolso` há 16 horas, sem erro. Estou verificando filas/SLA; não reprocessarei a CCB recusada.
9. **Status final:** EM ANÁLISE

## IT-2012 - Dúvida sobre prazo de desembolso

### Trilha de troubleshooting

1. **Onde consulto primeiro:** Sistema interno -> CPF `90212121212`, CCB `90012001` -> assinatura, validação bancária e desembolso.
2. **O que busco:** Horas desde assinatura, status do contrato/desembolso, erros e validação da conta.
3. **O que encontrei:** CSV: `contract_status=assinado`, `disbursement_status=aguardando_desembolso`, `disbursement_error` vazio, `last_disbursement_attempt=2026-08-06T12:15:00Z`, `bank_validated=sim`, `hours_since_signature=2`.
4. **Hipótese:** É uma espera de apenas 2 horas dentro do fluxo, sem falha identificada.
5. **Retry / reprocesso:** N/A - não há erro nem atraso comprovado que justifique liberar/reprocessar o PIX.
6. **Correção manual no sistema interno:** N/A - não liberar manualmente; orientar conforme o prazo oficial do desembolso.
7. **Escalação:** N/A - alterar o status de Crítico e acompanhar; escalar só se ultrapassar o SLA ou surgir erro.
8. **Comunicação no Jira:** `@agente.cx10` A CCB 90012001 está assinada, com conta validada, sem erro e há cerca de 2 horas em `aguardando_desembolso`. Orientei acompanhar o prazo operacional; não é necessário liberar PIX manualmente agora.
9. **Status final:** SOLUCIONADO