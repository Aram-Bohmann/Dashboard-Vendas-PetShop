# 📊 Dashboard - Análise Pet Shop

[![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=power-bi&logoColor=black)](https://powerbi.microsoft.com/)
[![DAX](https://img.shields.io/badge/DAX-Advanced-orange?style=for-the-badge)]()
[![ETL](https://img.shields.io/badge/ETL-Complete-success?style=for-the-badge)]()
[![Data Quality](https://img.shields.io/badge/Data_Quality-100%25-brightgreen?style=for-the-badge)]()

> **Projeto completo de governança de dados com ETL, tratamento de qualidade e dashboard analítico**  
> Relatório técnico desenvolvido no curso Técnico em Ciência de Dados - CEDUP Timbó

Pipeline completo de governança de dados aplicado a um Pet Shop fictício: desde a ingestão até análises estratégicas. Tratamento de 250.059 registros com 16 variáveis, correção de inconsistências, enriquecimento de dados e construção de dashboard interativo para tomada de decisão.

---

## 📖 Sobre o Projeto

Projeto acadêmico completo de **Governança de Dados** que demonstra todo o ciclo de vida de um projeto de Business Intelligence:

1. 📥 **Ingestão de Dados** - Análise inicial e dicionário de dados
2. 🧹 **Tratamento de Dados** - Limpeza e padronização
3. 🔧 **Manipulação de Dados** - Enriquecimento e medidas DAX
4. 📊 **Análise de Dados** - Dashboard e tomada de decisão

### 🎯 Objetivo

Transformar dados brutos e inconsistentes em informações estratégicas para otimizar o desempenho de vendas de um Pet Shop através de governança de dados aplicada.

---

## 📊 Dataset Original

### Características Iniciais

| Atributo | Valor |
|----------|-------|
| **Registros** | 250.059 linhas |
| **Variáveis** | 16 colunas |
| **Colunas Categóricas** | 4 (Forma Pagamento, Categoria, Estado, Região) |
| **Tipo Predominante** | Texto (7 colunas), Numérico (6 colunas) |
| **Período** | Dados de vendas anuais |

### 📋 Dicionário de Dados

<details>
<summary><b>16 Colunas Detalhadas</b></summary>

| Nome Original | Nome Tratado | Descrição | Tipo | Categórica | Valores Vazios |
|---------------|--------------|-----------|------|------------|----------------|
| CÓDIGO PEDIDO | Código Pedido | Código do pedido | Numérico | Não | Não |
| região-país | Região | Região do Brasil | Texto | Sim | Não |
| produto | Produto | Produto | Texto | Não | Não |
| valor | Valor Unitário | Valor unitário do produto | Numérico | Não | Não |
| quantidade | Quantidade | Quantidade do produto | Numérico | Não | Não |
| valor_total_bruto | Valor Bruto | Valor total bruto | Numérico | Não | Não |
| DIA | Dia | Dia da compra | Numérico | Não | Não |
| MES | Mês | Mês da compra | Numérico | Não | Não |
| ANO | Ano | Ano da compra | Numérico | Não | Não |
| estado | Estado | Estado brasileiro | Texto | Sim | Não |
| formapagto | Forma de Pagamento | Forma de pagamento | Numérico | Sim | Não |
| centro_distribuicao | Centro de Distribuição | Centro de distribuição | Texto | Não | Não |
| responsavelpedido | Responsável do Pedido | Responsável pelo pedido | Texto | Não | Não |
| valor_comissao | Valor da Comissão | Valor da comissão | Numérico | Não | Não |
| lucro_liquido | Lucro Líquido | Lucro líquido | Numérico | Não | Não |
| categoriaPROD | Categoria do Produto | Categoria do produto | Texto | Sim | Não |

</details>

---

## 🧹 Etapa 1: Tratamento de Dados

### Problemas Identificados e Soluções

#### 1️⃣ Nomenclatura de Colunas

**❌ Problema:** Headers incorretos na linha 1, dados reais na linha 3
```
Linha 1: [vazio] [vazio] [vazio]
Linha 2: [vazio] [vazio] [vazio]
Linha 3: CÓDIGO PEDIDO | região-país | produto
```

**✅ Solução:**
- Exclusão das linhas 1 e 2
- Promoção da linha 3 como cabeçalho
- Resultado: Headers corretos desde a linha 1

---

#### 2️⃣ Padronização de Nomenclatura

**❌ Problema:** Inconsistência nos nomes das colunas
```
região-país
valor_total_bruto
formapagto
categoriaPROD
```

**✅ Solução:** Padronização com "Título Com Espaços"
```
Região
Valor Bruto
Forma de Pagamento
Categoria do Produto
```

---

#### 3️⃣ Padronização de Valores

**❌ Problema:** Valores duplicados com grafias diferentes
```
Região: "centrooeste" vs "Centro-Oeste"
Estado: "saopaulo" vs "São Paulo"
Categoria: "bebedouro" vs "Bebedouro"
```

**✅ Solução:** Padronização completa de todos os valores categóricos
```
Região: "Centro-Oeste" (consistente)
Estado: "São Paulo" (consistente)
Categoria: "Bebedouros" (consistente)
```

---

#### 4️⃣ Formatação Numérica

**❌ Problema:** Ponto flutuante americano (.) ao invés de vírgula (,)
```
Tipo: String
Valor: "123.45"
```

**✅ Solução:**
- Substituição de `.` por `,`
- Conversão de String para Decimal
- Resultado: `123,45` (Decimal)

---

#### 5️⃣ Datas Fragmentadas

**❌ Problema:** Datas em 3 colunas separadas + datas inválidas
```
DIA: 31 | MES: 02 | ANO: 2023  ❌ (31/02 não existe)
DIA: 29 | MES: 02 | ANO: 2023  ❌ (2023 não é bissexto)
```

**✅ Solução:**
- Correção de datas inválidas para data mais próxima
- Mesclagem das 3 colunas: `DD/MM/YYYY`
- Conversão para tipo `Date`
- Resultado: Coluna única "Data" (tipo Date)

---

#### 6️⃣ Valores Negativos

**❌ Problema:** Valores numéricos negativos sem sentido de negócio
```
Quantidade: -5
Valor Bruto: -150,00
```

**✅ Solução:**
- Conversão para texto
- Remoção do sinal `-`
- Reconversão para decimal
- Resultado: Valores positivos consistentes

---

### 🎯 Estratégia para Outliers

**Problema identificado:** Valor Bruto de R$ 879.789 para produto de R$ 117

**Lógica de correção:**
```dax
Valor Bruto Corrigido = [Valor Unitário] * [Quantidade]
```

**Resultado:** 100% dos outliers corrigidos matematicamente

---

### 🔧 Estratégia para Missings

#### Missing em Quantidade

**❌ Problema:** 20 linhas com quantidade missing

**✅ Solução - Lógica de Dedução:**
```dax
Quantidade Corrigida = 
VAR ValorBrutoOriginal = [Valor Bruto]
VAR ValorUnitarioOriginal = [Valor Unitário]
RETURN
    IF(
        ISBLANK([Quantidade]),
        ROUND(ValorBrutoOriginal / ValorUnitarioOriginal, 0),
        [Quantidade]
    )
```

**Exemplo prático:**
```
Valor Unitário: R$ 10,00
Valor Bruto: R$ 40,00
Quantidade: [missing] → Calculado: 40 ÷ 10 = 4 unidades
```

---

#### Missing em Valor Bruto

**✅ Solução - Lógica Inversa:**
```dax
Valor Bruto Corrigido = 
VAR QuantidadeOriginal = [Quantidade]
VAR ValorUnitarioOriginal = [Valor Unitário]
RETURN
    IF(
        ISBLANK([Valor Bruto]),
        QuantidadeOriginal * ValorUnitarioOriginal,
        [Valor Bruto]
    )
```

**Resultado:** 100% dos missings resolvidos com lógica de negócio

---

## 🔧 Etapa 2: Manipulação e Enriquecimento

### 📊 Medidas DAX Criadas

#### KPIs Principais
```dax
// Quantidade Total Vendida
Quantidade Total = SUM(Vendas[Quantidade])

// Valor Bruto Total
Valor Bruto Total = SUM(Vendas[Valor Bruto])

// Lucro Líquido Total
Lucro Líquido Total = SUM(Vendas[Lucro Líquido])

// Margem de Lucro (%)
Margem de Lucro = 
DIVIDE(
    [Lucro Líquido Total],
    [Valor Bruto Total],
    0
) * 100

// Ticket Médio
Ticket Médio = 
DIVIDE(
    [Valor Bruto Total],
    DISTINCTCOUNT(Vendas[Código Pedido]),
    0
)
```

---

### 📅 Tabela Calendário (dCalendario)
```dax
dCalendario = 
ADDCOLUMNS(
    CALENDAR(
        DATE(2022, 1, 1),
        DATE(2024, 12, 31)
    ),
    "Ano", YEAR([Date]),
    "Mês Número", MONTH([Date]),
    "Mês Nome", FORMAT([Date], "MMMM"),
    "Trimestre", "T" & QUARTER([Date]),
    "Dia da Semana", WEEKDAY([Date]),
    "Nome Dia Semana", FORMAT([Date], "dddd")
)
```

**Relacionamento:** `dCalendario[Date]` → `Vendas[Data]` (1:N)

---

### 🗺️ Correção de Regiões por Estado
```dax
Região Corrigida = 
SWITCH(
    Vendas[Estado],
    "São Paulo", "Sudeste",
    "Rio de Janeiro", "Sudeste",
    "Minas Gerais", "Sudeste",
    "Espírito Santo", "Sudeste",
    "Rio Grande do Sul", "Sul",
    "Santa Catarina", "Sul",
    "Paraná", "Sul",
    "Bahia", "Nordeste",
    "Ceará", "Nordeste",
    "Pernambuco", "Nordeste",
    // ... (todos os 27 estados)
    "Indefinido"
)
```

---

### 🎯 Título Dinâmico por Ano
```dax
Título Dashboard = 
VAR AnoSelecionado = SELECTEDVALUE(dCalendario[Ano], "Todos")
RETURN
    "Desempenho de Vendas Pet Shop - " & AnoSelecionado
```

**Resultado:** Título se atualiza automaticamente ao filtrar ano

---

## 📊 Etapa 3: Dashboard Interativo

### 📸 Dashboard Final

<p align="center">
  <img width="100%" alt="Dashboard Power BI" src="https://github.com/user-attachments/assets/3ac818e1-b798-4970-b872-08c7bcbf3f86" />
</p>

---

### 🎨 Componentes do Dashboard

#### 📌 KPIs (Indicadores)
- 💰 **Lucro Total** - Soma do lucro líquido
- 🎫 **Ticket Médio** - Valor médio por pedido
- 📦 **Quantidade Vendida** - Total de unidades
- 📊 **Margem de Lucro (%)** - Rentabilidade percentual

#### 📈 Visualizações

| Tipo | Objetivo |
|------|----------|
| **Gráfico de Linhas** | Tendência de vendas mensal |
| **Gráfico de Barras** | Top distribuidoras, categorias e produtos |
| **Mapa Coroplético** | Performance regional por estado |
| **Cartões de Dica** | Detalhamento ao hover |

#### 🔍 Filtros Interativos
- 📂 Categoria do Produto
- 📅 Ano
- 📆 Mês
- 🗺️ Região
- 📍 Estado

---

## 💡 Insights e Tomada de Decisão

### 1️⃣ Desempenho Regional

**📊 Análise:**
- Estados como **Sergipe e RN** não têm clientes ativos
- Regiões com baixo volume identificadas no mapa

**🎯 Ações Recomendadas:**
✅ Intensificar marketing local  
✅ Ampliar rede de distribuidores  
✅ Promoções regionais de abertura  
✅ Adaptar produtos às necessidades locais  

---

### 2️⃣ Centro de Distribuição

**📊 Análise:**
- **Rapid Pink** e **Gold Beach** lideram vendas
- **Tree True** apresenta baixo desempenho

**🎯 Ações Recomendadas:**
✅ Revisar contratos com distribuidoras de baixa performance  
✅ Negociar melhores condições comerciais  
✅ Oferecer exclusividades de produtos  
✅ Implementar metas e incentivos  

---

### 3️⃣ Mix de Produtos

**📊 Análise:**
- **Bebedouros e Comedouros** lideram receita
- **Medicamentos e Alimentação** em seguida
- Categorias menos rentáveis identificadas

**🎯 Ações Recomendadas:**
✅ Aumentar estoque de categorias rentáveis  
✅ Campanhas de cross-selling  
✅ Kits promocionais (comedouro + ração)  
✅ Descontinuar produtos de baixo giro  

---

### 4️⃣ Tendência Temporal

**📊 Análise:**
- **Picos:** Fevereiro e Outubro
- **Quedas:** Junho a Agosto

**🎯 Ações Recomendadas:**
✅ Promoções sazonais nos meses fracos  
✅ Programas de fidelidade  
✅ Campanhas temáticas ("Inverno Pet", "Mês do Banho")  
✅ Descontos progressivos  

---

## 📊 Resultados Alcançados

### Qualidade de Dados

| Métrica | Antes | Depois | Melhoria |
|---------|-------|--------|----------|
| **Registros válidos** | ~249.000 | 250.059 | +1.059 |
| **Missings** | 20 linhas | 0 | 100% |
| **Outliers** | ~50 | 0 | 100% |
| **Datas inválidas** | ~100 | 0 | 100% |
| **Padronização** | 0% | 100% | ✅ |

### Enriquecimento

✅ **10 medidas DAX** criadas  
✅ **1 tabela calendário** implementada  
✅ **Relacionamentos** estabelecidos  
✅ **Colunas calculadas** para análises  

---

## 🛠️ Stack Tecnológica

### Ferramentas
![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=flat-square&logo=power-bi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-orange?style=flat-square)
![Power Query](https://img.shields.io/badge/Power_Query-blue?style=flat-square)

### Técnicas Aplicadas
- ✅ **ETL** - Extract, Transform, Load
- ✅ **Data Quality** - Limpeza e validação
- ✅ **Data Modeling** - Relacionamentos Star Schema
- ✅ **DAX** - Medidas e colunas calculadas
- ✅ **Data Visualization** - Dashboards interativos

---

## 🎓 Contexto Acadêmico

### Informações do Projeto

| Item | Detalhe |
|------|---------|
| **Curso** | Técnico em Ciência de Dados |
| **Instituição** | CEDUP Timbó - SC |
| **Ano** | 2025 |
| **Professor** | Wendell Thomas Teske |
| **Estudante** | Aram Bohmann Leite da Luz |
| **Diretora** | Jenifer Milena Pellin da Silva |

### Competências Demonstradas

1. **🧹 Qualidade de Dados** - Tratamento completo de inconsistências
2. **🔧 ETL** - Pipeline de transformação de dados
3. **📊 Modelagem de Dados** - Star schema e relacionamentos
4. **💻 DAX Avançado** - Medidas complexas e colunas calculadas
5. **📈 Data Visualization** - Dashboards profissionais
6. **💡 Business Intelligence** - Insights e tomada de decisão
7. **📝 Documentação** - Relatório técnico detalhado

---

## 📝 Licença

Este projeto foi desenvolvido para fins **acadêmicos** e está disponível para:

✅ Uso educacional e estudo  
✅ Referência técnica  
✅ Portfólio profissional  

---

## 📞 Contato

**Desenvolvedor:** Aram Bohmann Leite da Luz

[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:arambohmannleitedaluz@gmail.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/aram-luz-1b0ab1321)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Aram-Bohmann)
[![Portfolio](https://img.shields.io/badge/Portfolio-FF5722?style=for-the-badge&logo=google-chrome&logoColor=white)](https://aram-bohmann.github.io/Site-Portfolio/)

---

## 🙏 Agradecimentos

- **CEDUP Timbó** - Formação técnica de excelência
- **Prof. Wendell Thomas Teske** - Orientação no projeto
- **Power BI Community** - Recursos e tutoriais
- **Microsoft Learn** - Documentação DAX

---

<div align="center">

### ⭐ Se este projeto foi útil para você, considere dar uma estrela!

**Desenvolvido com 💙 e 📊 no CEDUP Timbó 2025**

*"Dados governados são decisões assertivas"*

📊 **Qualidade de Dados: 100%** | 🎯 **Insights Acionáveis** | 📈 **Dashboard Interativo**

</div>
```
