# Dashboard TechStore Brasil

Dashboard executivo interativo de vendas para a TechStore Brasil — loja de eletrônicos.

## Stack

- **Runtime:** Python 3.14 (`uv` via `.python-version`)
- **Framework:** Streamlit
- **Visualização:** Plotly Express / Plotly Graph Objects
- **Dados:** pandas + openpyxl (leitura do `.xlsx`)
- **Gerenciador de pacotes:** uv (pyproject.toml já existe)
- **Deploy alvo:** Streamlit Community Cloud (gratuito)

## Fonte de dados

Arquivo: `base_vendas_techstore.xlsx`

- 32.000 transações
- 18 meses de histórico
- Colunas esperadas (inferidas do domínio):
  - Data da venda
  - Produto / Categoria
  - Canal de Venda (ex: loja física, e-commerce, marketplace)
  - Região / Estado
  - Vendedor
  - Receita / Custo / Lucro
  - Forma de Pagamento
  - Quantidade

Toda agregação deve ser feita sobre os dados reais do arquivo. Números nunca devem ser inventados.

## Ponto de entrada

O app Streamlit deve ficar em `app.py` (ou substituir `main.py`). O comando de execução será `streamlit run app.py`.

## Funcionalidades obrigatórias

### KPI Cards (topo da página)
- Receita Total
- Lucro Total
- Margem de Lucro (%)
- Ticket Médio
- Total de Pedidos
- Cada card exibe variação vs. período anterior com seta e percentual

### Filtro global
- Seletor de período: 6 meses / 12 meses / Tudo
- Aplicado a todos os gráficos e KPIs simultaneamente

### Gráficos (8 obrigatórios)
1. Evolução mensal Receita vs Lucro — linha com dois eixos Y
2. Receita por Categoria — donut ou barras horizontais
3. Receita por Canal de Venda — barras verticais
4. Receita por Região — mapa choropleth ou barras
5. Top 10 Produtos mais vendidos — barras horizontais
6. Evolução da Margem de Lucro ao longo do tempo — área
7. Comparativo por Forma de Pagamento — donut
8. Ranking Top 5 Vendedores — barras horizontais

Cada gráfico deve ter: título claro + insight curto abaixo do gráfico.

### Tabela final
Resumo mensal: Receita, Custo, Lucro, Margem (%)

## Design system

### Cores
```
Fundo principal:   #0a0a0f
Cards:             #1a1a24
Borda sutil:       #2a2a38
Acento verde:      #00e676
Acento azul:       #2979ff
Acento laranja:    #ff9100
Texto primário:    #f0f0f8
Texto secundário:  #8888aa
```

### Tipografia
- Fonte: Inter (via CSS injection no Streamlit) ou fallback system-ui
- Hierarquia: título > subtítulo > label > valor > caption

### Componentes
- Border-radius: 12–16px em cards e gráficos
- Sombras suaves: `box-shadow: 0 4px 24px rgba(0,0,0,0.4)`
- Glow sutil em métricas de destaque
- Espaçamento generoso entre seções (não comprimir)
- Gradientes sutis nos backgrounds dos gráficos Plotly

### Interatividade
- Hover nos cards: leve elevação (transform + sombra via CSS)
- Tooltips ricos no Plotly: mostrar valor exato formatado em pt-BR
- Transições suaves (CSS `transition: 0.2s ease`)

## Formatação de números

Padrão brasileiro em todo o dashboard:
- Moeda: `R$ 1.234.567,89`
- Percentual: `12,34%`
- Inteiro: `1.234`

Usar `locale` ou função utilitária própria — não depender de locale do SO.

## Estrutura de arquivos esperada

```
dashboard-techstore-brasil/
├── app.py                        # ponto de entrada Streamlit
├── base_vendas_techstore.xlsx    # dados (não commitar se sensível)
├── pyproject.toml                # dependências via uv
├── .python-version               # 3.14
├── .gitignore
└── CLAUDE.md
```

## Restrições e boas práticas

- Não inventar dados — toda métrica vem do xlsx
- Não usar `st.cache_data` de forma que quebre o filtro de período
- Código sem comentários desnecessários; comentar apenas decisões não óbvias
- Não criar abstrações além do necessário para o dashboard funcionar
- O deploy no Streamlit Cloud exige `requirements.txt` ou `pyproject.toml` com dependências explícitas

## Footer

Exibir discretamente no rodapé: `Dados fictícios — gerado em [data atual]`
