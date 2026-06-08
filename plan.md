# Plano de Execução — Dashboard TechStore Brasil

---

## Fase 1 — Setup do ambiente

1.1 Adicionar dependências ao `pyproject.toml`:

- streamlit, plotly, pandas, openpyxl

1.2 Renomear/substituir `main.py` → `app.py` (ponto de entrada Streamlit)

1.3 Criar `.gitignore` adequado (ignorar `__pycache__`, `.venv`, possivelmente o `.xlsx` se sensível)

---

## Fase 2 — Carregamento e tratamento dos dados

2.1 Ler `base_vendas_techstore.xlsx` com pandas (`@st.cache_data` para performance)

2.2 Inspecionar colunas reais e mapear para os nomes internos usados no resto do código (ex: `data`, `receita`, `custo`, `lucro`, `categoria`, `canal`, `regiao`, `vendedor`, `pagamento`, `produto`, `quantidade`)

2.3 Garantir tipagem correta: datas como `datetime`, valores numéricos como `float`

2.4 Criar coluna `margem_pct = lucro / receita * 100` se não existir

2.5 Criar coluna `ano_mes` (período `YYYY-MM`) para agrupamentos mensais

---

## Fase 3 — Filtro global de período

3.1 Implementar seletor no topo: 6 meses / 12 meses / Tudo

3.2 Calcular data de corte com base no mês mais recente da base (não na data de hoje)

3.3 Aplicar filtro a um `df_filtered` que alimenta todos os KPIs e gráficos

3.4 Calcular `df_prev` (período anterior de mesmo tamanho) para os deltas dos KPIs

---

## Fase 4 — Injeção do tema dark mode

4.1 Usar `st.markdown` com `<style>` para aplicar:

- Background `#0a0a0f`, cards `#1a1a24`, bordas `#2a2a38`
- Fonte Inter via Google Fonts
- Border-radius 12–16px, sombras, hover nos cards
- Espaçamentos generosos entre seções

4.2 Configurar `config.toml` do Streamlit (`[theme]`) para base dark — evita flash de tema claro

---

## Fase 5 — Header

5.1 Título: `Dashboard TechStore`

5.2 Subtítulo dinâmico com o período selecionado (ex: `Jan 2024 – Jun 2024`)

---

## Fase 6 — KPI Cards (5 cards em linha)

Para cada KPI: Receita Total, Lucro Total, Margem %, Ticket Médio, Total de Pedidos

6.1 Calcular valor atual e valor do período anterior

6.2 Calcular delta percentual e definir seta (↑ verde / ↓ vermelho)

6.3 Renderizar via `st.columns(5)` com HTML customizado (card estilizado com glow)

6.4 Formatar todos os valores no padrão `R$ 1.234.567,89`

---

## Fase 7 — Os 8 gráficos

Cada gráfico seguirá o padrão: título + Plotly figure estilizada (dark, sem gridlines pesadas, tooltips ricos) + insight curto em texto abaixo.

| #  | Gráfico                          | Tipo                 | Layout              |
|----|----------------------------------|----------------------|---------------------|
| 1  | Evolução mensal Receita vs Lucro | Linha duplo eixo Y   | linha 1 (col cheia) |
| 2  | Receita por Categoria            | Donut                | col 1/2             |
| 3  | Receita por Canal de Venda       | Barras verticais     | col 2/2             |
| 4  | Receita por Região               | Barras horizontais¹  | linha cheia         |
| 5  | Top 10 Produtos mais vendidos    | Barras horizontais   | linha cheia         |
| 6  | Evolução da Margem de Lucro      | Área                 | linha cheia         |
| 7  | Forma de Pagamento               | Donut                | col 1/2             |
| 8  | Ranking Top 5 Vendedores         | Barras horizontais   | col 2/2             |

> ¹ Mapa choropleth será tentado com `plotly.express.choropleth` + GeoJSON do Brasil; se os dados de região forem por nome de estado, barras são o fallback seguro.

7.x Aplicar paleta de cores consistente (verde/azul/laranja em degradê) e template Plotly customizado reutilizável

---

## Fase 8 — Tabela de resumo mensal

8.1 Agrupar por `ano_mes`: Receita, Custo, Lucro, Margem %

8.2 Renderizar com `st.dataframe` estilizado (highlight de margem alta/baixa)

8.3 Ordenar do mais recente para o mais antigo

---

## Fase 9 — Footer

9.1 Texto discreto: `Dados fictícios — gerado em [data atual]`

---

## Fase 10 — Preparação para deploy

10.1 Confirmar que `pyproject.toml` tem todas as dependências com versões fixadas

10.2 Criar `requirements.txt` (o Streamlit Cloud aceita os dois, mas `.txt` é mais universal)

10.3 Criar `.streamlit/config.toml` com configurações de tema e layout wide

10.4 Testar `streamlit run app.py` localmente antes do deploy

---

## Ordem de implementação sugerida

Fase 1 → 2 → 3 → 4 → 5 → 6 → 7 (gráficos 1→8) → 8 → 9 → 10

Cada fase é testável individualmente — o app roda parcialmente a cada etapa.

---

Pode validar, ajustar a ordem, remover itens ou me dar mais contexto sobre as colunas reais do xlsx (especialmente nomes exatos) antes de eu começar a implementar.
