<div align="center">
  <img src="SG_Site.png" alt="Logo Silvanio Gois" width="160">
  <h1>Análise de Risco Climático e Modelagem Preditiva para Obras</h1>
  <p><b>Prova de Conceito (PoC) de Engenharia de Dados & Machine Learning aplicada à Gestão de Operações na Construção Civil</b></p>
</div>

---

## 1. Visão Geral do Projeto

Paralisações não planejadas causadas por intempéries climáticas (chuvas intensas, tempestades e rajadas de vento) representam um dos principais fatores de desvio de cronograma e custos imprevistos na engenharia de construção. A ociosidade de equipes mobilizadas, a locação ineficiente de maquinários pesados (como guindastes e bombas de concreto) e o retrabalho em etapas sensíveis (fundações e terraplenagem) geram perdas financeiras expressivas.

Esta **Prova de Conceito (PoC)** desenvolve uma solução completa de Inteligência de Dados para antecipar e mitigar esses riscos operacionais. O projeto consome séries temporais meteorológicas históricas reais da região da Baixada Santista/SP, aplica engenharia de atributos e treina um modelo preditivo baseado em **XGBoost** para classificar dias com risco de paralisação com antecedência, viabilizando decisões preventivas de alocação de recursos.

---

## 2. Escopo Geográfico e Fontes de Dados

* **Região Analisada:** Município de Cubatão e Baixada Santista, Estado de São Paulo, Brasil (`Latitude: -23.8931`, `Longitude: -46.4244`). Trata-se de uma zona costeira/industrial com alta variabilidade pluviométrica e elevada incidência de ventos fortes e temporais tropicais.
* **Origem dos Dados Meteorológicos:** Dados históricos de reanálise obtidos via integração com a **Open-Meteo Historical Weather API**.
* **Métricas Extraídas:**
  * Precipitação pluviométrica acumulada ($mm/h$).
  * Intensidade das rajadas de vento a 10 metros de altura ($km/h$).
  * Intervalo temporal: Séries horárias consolidadas cobrindo o período de 2022 a 2025.

---

## 3. Metodologia e Arquitetura da Solução

O fluxo metodológico foi estruturado em 4 etapas centrais:

1. **Ingestão e Tratamento de Dados (ETL):** Extração automatizada via requisições HTTP (`requests`), parsing de JSON e conversão em DataFrames com estampa de tempo sincronizada para o fuso horário local (`America/Sao_Paulo`).
2. **Estabelecimento de Limiares Operacionais de Paralisação:** Aplicação de critérios técnicos alinhados às normas de segurança e engenharia de campo:
   * **Critério de Chuva Crítica:** Precipitação horária $\ge 5.0\text{ mm/h}$.
   * **Critério de Vento Crítico:** Rajadas de vento $\ge 40.0\text{ km/h}$ (limite operacional de segurança para operação de guindastes e trabalho em altura).
3. **Análise Exploratória e Visualização de Negócio (EDA):**
   * Mapeamento da sazonalidade mensal de risco acumulado.
   * Diagnóstico do ciclo diurno (comportamento horário das 0h às 23h).
   * Matriz de risco bidimensional via *Heatmap* cruzando Horas do Dia x Meses do Ano.
4. **Modelagem Preditiva com Machine Learning (XGBoost):**
   * Agregação diária das variáveis meteorológicas e criação do *target* `dia_critico` ($\ge 3\text{ horas de paralisação/dia}$).
   * Treinamento do classificador `XGBClassifier` para inferência de risco diário.
   * Mapeamento da importância relativa dos atributos (*Feature Importance*).

---

## 4. Estrutura do Repositório

O repositório contém todos os artefatos de código, dados persistidos, relatórios exportados e visualizações geradas:

```text
EstudoMeteorologico/
│
├── AnaliseDeRiscoClimaticoParaObras.ipynb   # Notebook principal com o pipeline executável
├── AnaliseDeRiscoClimaticoParaObras.pdf     # Exportação em PDF do estudo para leitura executiva
├── AnaliseDeRiscoClimaticoParaObras.html    # Exportação interativa em HTML
├── dados_meteorologicos_brutos.csv          # Base bruta persistida extraída da API
│
├── ciclo_diurno_risco.png                   # Visualização: Distribuição de risco por hora
├── sazonalidade_mensal_alerta.png           # Visualização: Sazonalidade de alertas por mês
├── heatmap_risco_mes_hora.png               # Visualização: Matriz de calor Mês x Hora
├── feature_importance_xgboost.png           # Visualização: Importância de variáveis no modelo ML
├── SG_Site.png                              # Logotipo e identidade visual do projeto
└── Link para o projeto online               # https://silvaniosg.github.io/Analise-De-Risco-Climatico-Para-Obras/                          
```

---

## 5. Aplicação Prática no Processo Decisório (Casos de Uso)

A saída gerada pelo modelo permite transformar dados meteorológicos em decisões operacionais diretas no planejamento de obras:

* **Remanejamento Preventivo de Cronograma:** Identificação prévia de dias com alta probabilidade de chuva severa/vento para adiar etapas de concretagem ou elevação de estruturas, realocando equipes para tarefas internas e cobertas.
* **Otimização de Custos de Maquinário:** Redução de custos com a diária de equipamentos terceirizados parados por falta de condições climáticas.
* **Segurança do Trabalho:** Redução da exposição de colaboradores a cenários de risco extremo (raios e vendavais).

---

## 6. Tecnologias e Bibliotecas Utilizadas

* **Linguagem:** Python 3.x
* **Manipulação de Dados:** `pandas`, `numpy`
* **Requisição de APIs:** `requests`
* **Visualização de Dados:** `matplotlib`, `seaborn`
* **Machine Learning:** `scikit-learn`, `xgboost`

---

## 7. Recomendações para Trabalhos Futuros

* **Validação Temporal Cruzada:** Implementar `TimeSeriesSplit` para garantir validação puramente cronológica.
* **Engenharia de Recursos Avançada:** Adicionar variáveis de umidade relativa, pressão atmosférica e acumulado móvel de precipitação dos últimos 3 dias (saturação do solo).
* **Integração com RDO:** Acoplar os dados preditivos diretamente ao Relatório Diário de Obra (RDO) para calcular o impacto financeiro exato em Reais ($R\$$) decorrente da ociosidade.

---

## Autor

**Silvanio Gois**  
*Gestor de Operações e Negócios Orientado a Dados*

* **Estudo online:** [Acesso ao Estudo](https://silvaniosg.github.io/Analise-De-Risco-Climatico-Para-Obras/)
* **Website:** [www.silvaniogois.com.br](https://www.silvaniogois.com.br)
* **LinkedIn:** [linkedin.com/in/silvanio-gois](https://www.linkedin.com/in/silvanio-gois)
* **GitHub:** [github.com/SilvanioSG](https://github.com/SilvanioSG)
* **E-mail:** sg@silvaniogois.com.br
