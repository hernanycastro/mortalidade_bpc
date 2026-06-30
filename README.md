------------------------------------------------------------------------
# Ecossistema de Mortalidade BPC (2024) — Tábua de Sobrevivência Administrativa

[![R-v4.3+](https://img.shields.io/badge/R-v4.3%2B-blue.svg)](https://www.r-project.org/) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Repositório oficial do script de reprodutibilidade e artefatos analíticos do estudo **"Análise do Risco de Mortalidade entre beneficiários do BPC"**.

Este projeto unifica e agrega bases de dados administrativas do Governo Federal para estimar a função de sobrevivência e construir Tábuas de Mortalidade específicas para os beneficiários do Benefício de Prestação Continuada (BPC) no ano-base de 2024, introduzindo o conceito atuarial de **Sobrevivência Administrativa**.

------------------------------------------------------------------------

\## 📊 Resumo Metodológico

Para mitigar os vieses de contagem repetida e inflação artificial de sobrevivência decorrentes do formato de painel longitudinal mensal da folha de pagamentos (Base Maciça), este ecossistema adota as seguintes características metodológicas:

1.  **Pessoas-Ano sob Risco:** O denominador de exposição é calculado ponderando o tempo real de permanência ativa de cada registro ao longo das competências mensais do ano (estoque mensal dividido por 12).
2.  **Transformação de Greville (**$m_x \to q_x$): Conversão estrita da Taxa Central de Mortalidade ($m_x$) para a Probabilidade Real de Morte ($q_x$), com fechamento biológico obrigatório na idade limite de 100 anos.
3.  **Triangulação Nacional:** Validação cruzada e cálculo de Razão de Risco ($HR$) e Razão de Mortalidade Padronizada ($SMR$) utilizando como referências demográficas as projeções oficiais do **IBGE**, o Sistema de Informações sobre Mortalidade (**SIM/MS**) e o Sistema Nacional de Informações de Registro Civil (**SIRC**).
4.  **Validação por Machine Learning:** Teste de estresse estrutural e consistência da assinatura de risco da coorte assistencial por meio do algoritmo *Random Survival Forests* (RSF).

------------------------------------------------------------------------

\## 📁 Estrutura do Repositório

```text
├── data/
│   ├── bpc_agregado_2024.csv         # Matriz populacional anonimizada (LGPD)
│   ├── projecoes_quinquenal.xlsx     # Projeções populacionais oficiais do IBGE
│   ├── base_sim.csv                  # Microdados agregados de referência do SIM
│   └── base_sirc.csv                 # Microdados agregados de referência do SIRC
├── src/
│   └── script_reprodutibilidade.R    # Pipeline principal integrado de processamento
├── outputs/
│   ├── Figura2_Curvas_Sobrevivencia.png
│   ├── Figura3_Hazard_Ratio.png
│   ├── Figura4_SMR_Barras.png
│   └── Outputs_Mortalidade_BPC_2024.xlsx # Planilha multi-aba com os resultados exatos
└── README.md
```

\## 🛠️ Pré-requisitos e Dependências 

O script utiliza o gerenciador de pacotes pacman para automatizar a instalação das dependências. Certifique-se de ter o R instalado (versão 4.3 ou superior recomendada).

As principais bibliotecas utilizadas são:

- Manipulação de dados: data.table, dplyr, tidyr, lubridate, janitor

- Atuária e Machine Learning: survival, randomForestSRC, broom

- Visualização e Saídas: ggplot2, scales, readxl, writexl

\## 🚀 Como Executar

1.  Clonar o Repositório

git clone <https://github.com/seu-usuario/mortalidade-bpc.git> cd mortalidade-bpc

1.  Portabilidade

Abra o RStudio e configure o diretório de trabalho na raiz do projeto (ou utilize o sistema de `.Rproj`). O script foi desenhado de forma portátil, eliminando caminhos absolutos e criando automaticamente pastas de saídas necessárias.

1.  Execução

Execute o arquivo `src/script_reprodutibilidade.R`. O pipeline gerará automaticamente todas as tabelas atuariais e as figuras de alta resolução (300 DPI) na pasta de saídas.

\## 📈 Principais Indicadores Gerados

O pipeline reproduz os resultados discutidos no manuscrito, evidenciando o **Paradoxo da Longevidade Administrativa**:

- **SMR Geral (Ref: IBGE 2024):** Reduções drásticas nos riscos ponderados, fixando-se em **0,17** (Idoso Feminino), **0,22** (Idoso Masculino), **0,77** (PCD Feminino) e **0,70** (PCD Masculino).

- **Expectativa de Vida ao Nascer (**$e_0$): Calibrada para limitesde **82,2 anos** (PCD Feminino) e **79,2 anos** (PCD Masculino).

- **C-Index RSF:** Validação preditiva de **0,4277**, comprovando numericamente a inversão e a residualidade da assinatura de risco administrativa frente às tábuas biométricas nacionais.

\## ✍️ Autores

Wederson Santos - santoswederson1983@gmail.com
Hernany Gomes de Castro - hernany1@gmail.com

\## 📄 Licença
Este projeto está licenciado sob a Licença MIT. Os dados e o script foram disponibilizados com o objetivo de reprodutibilidade do estudo **"Análise do Risco de Mortalidade entre beneficiários do BPC"** (manuscrito submetido para publicação). A reprodução é permitida desde que citada a fonte.
