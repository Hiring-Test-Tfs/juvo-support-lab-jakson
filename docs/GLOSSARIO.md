# Glossário - termos do dia a dia

**Navegação:** [Índice](../INDICE.md) · [Fila](../FILA.md) · [Fluxo IT-Support](./FLUXO-IT-SUPPORT.md) · [Export CSV](../data/sistema-interno-export.csv)

Siglas e termos que aparecem nos tickets e no [export do sistema interno](../data/sistema-interno-export.csv).

| Termo | Significado |
|-------|-------------|
| **IT-Support** | Formulário no Slack/Jira onde o time de CX abre solicitações para o suporte de sistemas |
| **Sistema interno** | Ferramenta interna para consultar contratos, parcelas, pagamentos e status de operações |
| **CCB** | Número do contrato de crédito |
| **Desembolso** | Transferência do valor do empréstimo para a conta do cliente (PIX) |
| **Bancarizador** | Parceiro externo que executa o pagamento do desembolso |
| **Antifraude** | Motor de risco que pode **reter** um desembolso (`ANTIFRAUD_HOLD`) até análise |
| **Alpha9** | Etapa de análise de crédito / aprovação na jornada |
| **Baixar parcela** | Registrar no sistema que o cliente pagou (quitar ou abater parcela) |
| **Quitação** | Boleto/PIX do valor total para encerrar o contrato |
| **Termo de quitação** | Documento enviado por e-mail após o contrato ser quitado |
| **Juvo Negocia / Renegociação** | Fluxo de negociação de contrato em atraso |
| **Banco** | Instituição / provedor de cobrança (boletos/PIX de parcelas) |
| **Bounce** | E-mail rejeitado pelo provedor (endereço inválido ou caixa cheia) |
| **Opt-out** | Cliente pede para parar comunicações (SMS/e-mail), sem necessariamente exclusão LGPD completa |

**Status comuns de desembolso:** `aguardando_desembolso`, `desembolsado`, `indeferido`, `cancelado`, `erro_bancarizador`

**Status comuns de contrato:** `assinatura_pendente`, `assinado`, `recusado`, `cancelado`, `quitado`, `ativo`
