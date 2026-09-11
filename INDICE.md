# Índice - material do teste

**Navegação rápida**

| Passo | Link | O que é |
|-------|------|---------|
| **1. Entender o cenário** | [README](./README.md) | Regras, tempo, entrega |
| **2. Glossário** | [docs/GLOSSARIO.md](./docs/GLOSSARIO.md) | Termos do dia a dia |
| **3. Fluxo** | [docs/FLUXO-IT-SUPPORT.md](./docs/FLUXO-IT-SUPPORT.md) | Como o IT-Support trabalha |
| **4. Fila** | [FILA.md](./FILA.md) | Os 12 tickets da manhã + links |
| **5. Dados** | [data/sistema-interno-export.csv](./data/sistema-interno-export.csv) | Export fictício (cruzar com CPF/CCB) |
| **6. Modelo** | [docs/MODELO-TRILHA.md](./docs/MODELO-TRILHA.md) | Exemplo de trilha (passos 1-9) |
| **7. Entregar** | [ENTREGA.md](./ENTREGA.md) | O que criar na raiz do repo |
| **8. Reverse + IA** | [docs/REVERSE.md](./docs/REVERSE.md) | Pergunta reversa + declaração de IA |

## Material de leitura (passos 1-6)

| # | Arquivo | Uso |
|---|---------|-----|
| 1 | [README](./README.md) | Cenário e regras |
| 2 | [Glossário](./docs/GLOSSARIO.md) | CCB, desembolso, bancarizador… |
| 3 | [Fluxo IT-Support](./docs/FLUXO-IT-SUPPORT.md) | Triagem, resposta, quando escalar |
| 4 | [Fila do dia](./FILA.md) | Tabela + links dos tickets |
| 5 | [Export CSV](./data/sistema-interno-export.csv) | Estado plantado dos clientes |
| 6 | [Modelo de trilha](./docs/MODELO-TRILHA.md) | Formato esperado em `TRILHAS.md` |

## Tickets (IT-2001 … IT-2012)

- [IT-2001 - Desembolso (antifraude)](./tickets/IT-2001.md)
- [IT-2002 - Onboarding KYC](./tickets/IT-2002.md)
- [IT-2003 - Pagamento (PIX duplicado)](./tickets/IT-2003.md)
- [IT-2004 - Boleto vencido](./tickets/IT-2004.md)
- [IT-2005 - Renegociação (valor)](./tickets/IT-2005.md)
- [IT-2006 - Parar SMS](./tickets/IT-2006.md)
- [IT-2007 - Termo de quitação](./tickets/IT-2007.md)
- [IT-2008 - Desembolso (CCB recusada)](./tickets/IT-2008.md)
- [IT-2009 - Pagamento (valor divergente)](./tickets/IT-2009.md)
- [IT-2010 - Pagamento (alega quitação)](./tickets/IT-2010.md)
- [IT-2011 - Desembolso (2ª CCB)](./tickets/IT-2011.md)
- [IT-2012 - Dúvida prazo](./tickets/IT-2012.md)

## Entrega

| Arquivo | Conteúdo | Onde |
|---------|----------|------|
| `TRIAGEM.md` | Ordem dos 12 tickets + justificativa curta | Criar na **raiz** do repo |
| `TRILHAS.md` | Passo a passo em **cada** ticket | Criar na **raiz** - **principal** |
| [docs/REVERSE.md](./docs/REVERSE.md) | Pergunta reversa + declaração de IA | **Preencher** o template |

### [Pergunta reversa + IA](./docs/REVERSE.md) - resumo

**Por quê:** no suporte sênior você precisa saber quando o ticket ou o dado **não fecha** e **o que perguntar** ao time antes de escalar.

**O que fazer:** escolha **um** ponto do teste, escreva a **pergunta que faria à Juvo** e explique **como a resposta mudaria sua trilha**. Depois declare se usou IA (permitido - seja transparente).
