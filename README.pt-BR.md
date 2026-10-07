# Otimização Multiobjetivo de Redes Urbanas de Pluviômetros

**Realocação de estações de monitoramento de chuva integrando Geoprocessamento e Pesquisa Operacional**

🌐 **Idioma / Language:** **Português** | [English](README.md)

---

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)
[![Solver](https://img.shields.io/badge/Solver-PuLP%20%2F%20CBC-success.svg)]()
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-pesquisa-blueviolet.svg)]()

> Parte da pesquisa de doutorado em Pesquisa Operacional na **UNIFESP / ITA**.
> Veja a visão geral da pesquisa: [PhD-Research-Operational-Research](https://github.com/roberval1994).

## Visão geral

Este projeto propõe e compara metodologias para **realocar e otimizar estações
pluviométricas** na cidade de São José dos Campos (SP, Brasil). Integra
**geoprocessamento** e **pesquisa operacional** para maximizar a eficiência da rede sobre
áreas críticas, guiado por um **peso multiobjetivo** (`W_exp`) que combina
**densidade populacional**, **zonas de risco hidrológico** e **volume de precipitação**.

## Formulação do problema

Cada ponto candidato em um grid espacial recebe uma pontuação composta derivada de três
camadas. A otimização então seleciona as localizações de estação que maximizam o peso coberto.

| Camada | Fonte | Processamento |
|---|---|---|
| **Chuva** | Dados reais de 2025 (PlugField) | Remoção de outliers de estações com falha |
| **Risco** | Polígonos de inundação / geológicos (GeoSanja/SJC) | Buffer métrico de 800 m para incerteza espacial |
| **População** | WorldPop 2025 (resolução 100 m) | População acumulada em raio de 1.000 m |

A precipitação é interpolada por **Krigagem Ordinária** para gerar uma superfície contínua
de chuva que alimenta o modelo de otimização.

## Abordagens de otimização comparadas

1. **K-Means Ponderado (baseline)** — regionaliza o território pela densidade de peso e posiciona estações nos centroides dos clusters.
2. **Heurística gulosa com inibição espacial** — seleciona iterativamente os pontos de maior peso, impondo distância mínima (1,5 km) para evitar redundância.
3. **Modelo exato de Cobertura Máxima (MCLP)** — Programação Linear Inteira (PuLP / CBC) que encontra a cobertura globalmente ótima dentro de um raio de atendimento fixo (800 m).
4. **Algoritmos híbridos (decomposição espacial)**
   - *Híbrido 1*: clustering K-Means + heurística gulosa por região.
   - *Híbrido 2*: clustering K-Means + MCLP exato por sub-região (precisão do modelo exato com a velocidade da decomposição).

## Cenários & métricas

Os cenários testam diferentes equilíbrios de prioridade — **Equilibrado**,
**Foco em Risco**, **Foco em População**. O desempenho é avaliado por:

- Ganho de população coberta (**Pop %**)
- Ganho de risco coberto (**Risco %**)
- Pontuação total
- Tempo de execução (s)

Mapas interativos em **Folium** renderizam heatmaps de risco e os raios de cobertura das estações.

## Estrutura do repositório

```
.
├── notebooks/
│   ├── Projeto_Artigo.ipynb                  # Pipeline principal de modelagem e otimização
│   └── Artigo_Otimizacao_Pluviometros.ipynb  # Experimentos e comparações
├── data/                                     # Grids processados (.parquet), pesos (.csv)
├── maps/                                     # Mapas interativos Folium (.html)
├── results/                                  # Tabelas de métricas e saídas comparativas
├── requirements.txt
├── LICENSE
├── README.md                                 # Inglês
└── README.pt-BR.md                           # Português (este arquivo)
```

## Dados

> **Dados pesados** (ex.: o raster de população WorldPop, ~457 MB) **não são versionados**
> neste repositório. Veja [`Dados/LEIA-ME-dados.md`](Dados/LEIA-ME-dados.md) para instruções de download,
> fontes (WorldPop, GeoSanja/SJC, PlugField) e a estrutura esperada de pastas.

## Como começar

```bash
git clone https://github.com/roberval1994/Multiobjective-Optimization-of-Urban-Rain-Gauge-Networks.git
cd Multiobjective-Optimization-of-Urban-Rain-Gauge-Networks

python -m venv .venv
.venv\Scripts\activate          # Windows
# source .venv/bin/activate     # Linux / macOS
pip install -r requirements.txt

jupyter notebook
```

## Principais resultados

- Quatro estratégias de otimização comparadas sob três cenários de prioridade.
- O MCLP exato fornece o baseline de cobertura ótima; os híbridos recuperam a maior parte do ganho em uma fração do tempo.
- Mapas interativos tornam auditáveis os trade-offs de cobertura e risco.

## Tecnologias

`Python` · `GeoPandas` · `Shapely` · `PyKrige` · `PuLP` (CBC) · `scikit-learn` (K-Means) · `Folium` · `pandas` · `NumPy`

## Autor

**Roberval Gonçalves Moreira Filho**
Cientista de Dados | Analista de Pesquisa Operacional — Doutorando, UNIFESP/ITA

[![Email](https://img.shields.io/badge/Email-roberval.researcher.or%40outlook.com-red)](mailto:roberval.researcher.or@outlook.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-robervalOr-blue)](https://www.linkedin.com/in/robervalOr)
[![GitHub](https://img.shields.io/badge/GitHub-roberval1994-black)](https://github.com/roberval1994)

## Licença

Distribuído sob a Licença MIT. Veja [LICENSE](LICENSE).
