# Modelo - trilha de troubleshooting

**Navegação:** [Índice](../INDICE.md) · [Fila](../FILA.md) · [Export CSV](../data/sistema-interno-export.csv) · [Entrega](../ENTREGA.md)

Para **cada** ticket, documente o **caminho que você seguiria na prática** - não só a resposta final.

Neste teste você não tem sistema interno, Banco (cobrança) nem fornecedor reais. Use o [export do sistema interno](../data/sistema-interno-export.csv) como se fosse a primeira consulta. Nos passos, deixe claro o que faria **no ambiente real** (sistema interno, consulta read-only em banco de dados, reprocesso, etc.).

---

## Esqueleto sugerido (adapte por caso)

```
1. Onde consulto primeiro     → sistema interno / export / logs / fila de desembolso…
2. O que busco                → CPF, CCB, status, último erro, pagamento…
3. O que encontrei            → evidência (campo do CSV neste teste)
4. Hipótese                   → causa provável em 1 linha
5. Ação - retry/reprocesso    → se aplicável; senão "não aplicável" + por quê
6. Ação - correção manual     → baixa, cancelar boleto, reenviar termo…
7. Escalação                  → dev / antifraude / cobrança / ninguém + contexto
8. Comunicação                → comentário que postaria no Jira (@agente)
9. Status final               → SOLUCIONADO / EM ANÁLISE / ESCALADO
```

Nem todo passo existe em todo ticket. **Pule com "N/A"** e explique em uma linha.

---

## Exemplo (IT-2004 - boleto vencido)

**1. Onde consulto primeiro**  
Sistema interno → contrato 90004001 / CPF 902.444.444-04 → aba documentos/cobranças e parcelas.

**2. O que busco**  
Parcela 4 aberta? Documento ativo? Status do boleto (válido vs vencido)?

**3. O que encontrei**  
CSV: `installment_ref=parcela_4`, `installment_status=aberta`, `installment_amount_brl=412.50`, `last_doc_generated=doc_r2_0401`, `last_doc_type=boleto_parcela`, `last_doc_status=vencida`.

**4. Hipótese**  
Boleto da parcela 4 está vencido; cliente precisa de documento válido com novo vencimento.

**5. Retry/reprocesso**  
N/A - não é falha de job; é documento vencido.

**6. Correção manual**  
Cancelar/inativar `doc_r2_0401` e gerar novo boleto da parcela 4 com vencimento atual (R$ 412,50).

**7. Escalação**  
N/A - regenerar boleto é ação operacional do suporte.

**8. Comunicação**  
`@agente.cx04` Boleto vencido cancelado. Gere / enviei boleto válido da parcela 4 (R$ 412,50). Anexaria print do documento novo.

**9. Status final**  
SOLUCIONADO

---

Use este nível de detalhe nos 12 tickets em `TRILHAS.md` (ver [ENTREGA.md](../ENTREGA.md)).
