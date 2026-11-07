# 💼 Dashboard Comercial Executivo: Case Power BI Sênior

Solução de BI de ponta a ponta para a área comercial do **Grupo Omnilog**, uma empresa fictícia de distribuição: modelo dimensional, DAX avançado, segurança por linha e um protótipo web interativo.

> ⚠️ **Todos os números são fictícios**, gerados de forma determinística e reproduzível só para demonstrar as técnicas.

**[▶ Abrir o protótipo interativo](https://maatths.github.io/case-powerbi-comercial/)**

---

## 📦 O que tem neste repositório

| Pasta / arquivo | Conteúdo |
|---|---|
| [`index.html`](index.html) | Protótipo interativo do painel executivo. Abre direto no navegador e usa o mesmo gerador de dados do modelo |
| [`powerbi/Omnilog.pbip`](powerbi/Omnilog.pbip) | Projeto Power BI (PBIP): modelo em TMDL e relatório em PBIR, em texto versionável |
| `powerbi/Omnilog.SemanticModel/` | Tabelas, relacionamentos, medidas DAX e função de RLS |
| `powerbi/Omnilog.Report/` | Páginas Visão C-Level (27 visuais), Detalhamento Regional e Tooltip Filial |
| `powerbi/dados_csv/` | Dados fictícios em CSV (vendas, metas, filiais, produtos, regionais e segurança) |
| `powerbi/Tema_Omnilog_Corporativo.json` | Tema do relatório |

## 🧱 Modelo de dados

Modelo estrela com **duas tabelas fato de granularidades diferentes**, ligadas por **dimensões conformadas**:

```
            d_Calendario
                 │
 d_Filial ── f_Vendas ── d_Produto          f_Metas (regional × categoria × mês)
                 │   \                         │        │
           d_Regional  d_Categoria ────────────┘────────┘
```

- `f_Vendas`: vendas por item e dia (cerca de 22 mil linhas, 2023–2024).
- `f_Metas`: metas mensais por regional e categoria.
- `d_Regional` e `d_Categoria` filtram as duas fatos sem relação muitos-para-muitos, sem filtro bidirecional e sem snowflake.
- `d_Calendario` em DAX, que se ajusta ao intervalo dos dados a cada atualização, com flag de dia útil.
- `Seguranca_RLS`: tabela desconectada, lida via DAX (RLS dinâmico).

## 🧮 DAX em destaque (mais de 50 medidas)

- **Inteligência de tempo:** YTD, ano anterior, YoY %, média móvel de 3 meses (exibida só com 3 meses completos) e comparação de margem com o trimestre e o ano anterior.
- **Meta:** distribuída pelos **dias úteis** do mês dentro do período filtrado. No mês fechado, dá exatamente a meta cadastrada. No nível de filial fica em branco de propósito, porque a meta não existe nesse grão.
- **Participação da filial na regional:** duas versões, `REMOVEFILTERS` (total da regional) e `ALLSELECTED` (total da seleção), com `KEEPFILTERS(TREATAS(...))`.
- **Ranking, status e cores condicionais**, títulos dinâmicos e uma **narrativa executiva** gerada por medida.

## 🔒 Segurança e entrega

- **RLS dinâmico por regional:** o usuário vê só a regional da tabela de segurança, a diretoria vê tudo e quem não está cadastrado não vê nada (seguro por padrão).
- **Atualização incremental** (parâmetros `RangeStart` e `RangeEnd`), drill-through, bookmarks (gráficos ou tabela) e tooltip por filial.
- **Duas fontes de dados por parâmetro (`FonteDados`):** `CSV` (padrão, sem banco) ou `SQL` (views `bi.*` no SQL Server, com query folding). Trocar de fonte não exige mexer em medidas nem em relacionamentos.

## ▶️ Como abrir no Power BI Desktop

1. Clone ou baixe o repositório.
2. Abra `powerbi/Omnilog.pbip`.
3. Em *Transformar dados > Gerenciar parâmetros*, ajuste **`PastaDadosCSV`** para a pasta `powerbi/dados_csv` na sua máquina. O padrão é `C:\Dados\Omnilog\dados_csv`.
4. Clique em **Atualizar**. Para testar o RLS, use *Modelagem > Exibir como* e informe um dos e-mails fictícios de `Seguranca_RLS.csv`.

## 🔢 Números da base fictícia

| Item | Valor |
|---|---|
| Receita líquida total (2023–2024) | R$ 50,7 mi |
| Crescimento 2024 vs 2023 | +10,2% |
| Atingimento da meta 2024 | 104,9% (Nordeste lidera com 109,4%) |
| Maior queda de margem T3→T4/2024 | Norte: −9,3 p.p. |

## 🛠️ Tecnologias

`Power BI` · `DAX` · `Power Query (M)` · `TMDL/PBIR` · `SQL Server` · `RLS` · `HTML/CSS/JavaScript`

---

👤 **Matheus Oliveira**: [LinkedIn](https://www.linkedin.com/in/matheus-oliveira-06b0b5162) · [GitHub](https://github.com/Maatths)
