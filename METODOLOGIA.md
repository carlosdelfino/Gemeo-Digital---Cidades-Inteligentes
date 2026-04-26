![visitors](https://visitor-badge.laobi.icu/badge?page_id=carlosdelfino.Gemeo-Digital---Cidades-Inteligentes.METODOLOGIA)
[![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC_BY--SA_4.0-blue.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
![Language: Portuguese](https://img.shields.io/badge/Language-Portuguese-brightgreen.svg)
![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange)
![Machine Learning](https://img.shields.io/badge/Machine%20Learning-Prática-green)
![Status](https://img.shields.io/badge/Status-Educa%C3%A7%C3%A3o-brightgreen)
![Repository Size](https://img.shields.io/github/repo-size/carlosdelfino/Gemeo-Digital---Cidades-Inteligentes)
![Last Commit](https://img.shields.io/github/last-commit/carlosdelfino/Gemeo-Digital---Cidades-Inteligentes)

<!-- Animated Header -->
<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f172a,50:1a56db,100:10b981&height=220&section=header&text=Metodologia%20Canvas&fontSize=42&fontColor=ffffff&animation=fadeIn&fontAlignY=35&desc=Guia%20para%20Preenchimento%20do%20Canvas%20de%20IA&descSize=18&descAlignY=55&descColor=94a3b8" width="100%" alt="Metodologia Canvas Header"/>
</p>

## Visão Geral

Este documento fornece um guia metodológico estruturado para preenchimento do Canvas de Estrutura do Projeto de IA, baseado nos 4 checkpoints fundamentais descritos no material "Como Estruturar e Avaliar sua Solução de IA". A metodologia conecta cada card do canvas aos checkpoints, garantindo coerência e profundidade na construção da solução.

## Estrutura dos Checkpoints

O processo de desenvolvimento é organizado em 4 checkpoints sequenciais:

- **Checkpoint 1 - Problema**: O Ponto de Partida Inegociável
- **Checkpoint 2 - Modelagem**: Conectando Dados à Abordagem
- **Checkpoint 3 - MVP**: Algo que Funciona Mesmo que Simples
- **Checkpoint 4 - Avaliação**: A Etapa que Fecha o Ciclo

---

## Checkpoint 1 - Problema

### Cards Relacionados: 1 e 2

#### Card 1: Problema de Negócio

**Pergunta Guia:** O que estamos tentando resolver? Por que isso importa?

**Diretrizes do Checkpoint 1:**

- **Descreva o problema no contexto real:**
  - Quem sofre com este problema?
  - Em qual escala ele ocorre?
  - Qual é o custo de não resolvê-lo?

- **Dica Prática:**
  - Escreva o problema em uma frase
  - Se precisar de mais de duas linhas para explicar, ainda não está claro o suficiente

- **O que incluir:**
  - Contexto organizacional ou cenário real
  - Impacto atual do problema (custo, tempo, qualidade, risco)
  - Benefício potencial da solução
  - Stakeholders envolvidos

**Exemplo de Resposta Adequada:**
"O sistema de transporte urbano apresenta 30% de atrasos diários nos horários de pico, afetando 500.000 usuários e gerando perdas econômicas estimadas em R$ 2 milhões por mês devido à produtividade reduzida."

#### Card 2: Pergunta Analítica

**Pergunta Guia:** Qual pergunta o modelo precisa responder?

**Diretrizes do Checkpoint 1:**

- **Traduza o problema em uma pergunta analítica clara**
- A pergunta deve ter uma resposta que a IA possa calcular
- Não é uma reflexão filosórica, mas uma questão mensurável

- **Características de uma boa pergunta:**
  - Específica e mensurável
  - Ligada diretamente ao problema de negócio
  - Respondível com dados disponíveis
  - Com escopo bem definido

**Exemplo de Resposta Adequada:**
"Qual é a probabilidade de um veículo usar esta rota entre 18h e 20h?"

**Exemplos de Perguntas Inadequadas:**
- "Como podemos melhorar o trânsito?" (muito vaga)
- "O trânsito é bom ou ruim?" (subjetiva)
- "Qual o sentido da vida?" (não respondível por IA)

---

## Checkpoint 2 - Modelagem

### Cards Relacionados: 3, 4 e 5

#### Card 3: Variável-Alvo

**Pergunta Guia:** O que exatamente estamos prevendo/inferindo?

**Diretrizes do Checkpoint 2:**

- **A variável-alvo é o coração do projeto**
- Tudo é construído em torno dela
- Define o que o modelo deve prever, classificar ou detectar

- **O que especificar:**
  - Nome da variável
  - Tipo (binária, contínua, categórica)
  - Como ela é definida no dataset
  - Escala de valores possíveis

**Exemplos:**
- "Probabilidade de atraso (contínua, 0-1)"
- "Classe de tráfego (categórica: baixo/médio/alto)"
- "Presença de incêndio (binária: sim/não)"

#### Card 4: Dados Disponíveis

**Pergunta Guia:** Quais dados temos? Quais limitações existem?

**Diretrizes do Checkpoint 2:**

- **Avalie viabilidade antes de avançar**
- Volume, qualidade e formato precisam ser considerados

- **O que documentar:**
  - Fontes de dados (sistemas, sensores, APIs)
  - Volume de dados (número de registros, período coberto)
  - Granularidade (temporal, espacial)
  - Formato e estrutura
  - Viéses conhecidos
  - Dados faltantes
  - Restrições de acesso ou privacidade

- **Perguntas críticas:**
  - Os dados disponíveis suportam a abordagem escolhida?
  - A qualidade dos dados é adequada?
  - Há viéses que podem afetar o modelo?

**Exemplo de Resposta:**
"Dados históricos de GPS de 50.000 veículos (2020-2024), atualização a cada 30 segundos. Limitações: 15% de dados faltantes em áreas periféricas, viés de sub-representação de veículos de transporte de carga."

#### Card 5: Estratégia de IA

**Pergunta Guia:** Qual abordagem será usada? (Por quê?)

**Diretrizes do Checkpoint 2:**

- **A escolha deve ser justificada pelos dados disponíveis e pelo tipo de resposta esperada**
- Classificação? Regressão? Detecção de objetos?

- **O que especificar:**
  - Tipo de aprendizado (supervisionado, não supervisionado, reinforcement learning)
  - Família de algoritmos considerados
  - Justificativa para a escolha
  - Alinhamento com a variável-alvo e dados disponíveis

**Exemplo de Resposta:**
"Aprendizado supervisionado com regressão logística e random forest. Justificativa: variável-alvo binária, dados rotulados disponíveis, necessidade de interpretabilidade para stakeholders não técnicos."

---

## Checkpoint 3 - MVP

### Cards Relacionados: 7 (Resultado Esperado)

#### Card 7: Resultado Esperado

**Pergunta Guia:** O que queremos gerar?

**Diretrizes do Checkpoint 3:**

- **Funciona > Perfeito**
- O MVP é válido quando demonstra viabilidade técnica
- Não precisa ser robusto, mas precisa ser uma prova de conceito

- **Características do MVP válido:**
  - Demonstra que a solução é tecnicamente viável
  - Gera um resultado relevante para o problema definido
  - Pode ser simples, mas funcional
  - Tem output interpretável ligado ao problema

- **O que não é um MVP válido:**
  - Notebook com código rodando sem output interpretável
  - Modelo treinado sem conexão clara com o problema
  - Experimento isolado sem contexto de aplicação

- **O que especificar:**
  - Tipo de entrega (dashboard, modelo em produção, relatório, API, visualização)
  - Escopo do MVP (funcionalidades mínimas)
  - Como o resultado será consumido
  - Formato da saída

**Exemplo de Resposta:**
"API REST que retorna probabilidade de congestionamento para rotas específicas com horário de consulta. MVP inclui 5 rotas principais, atualização em tempo real, interface JSON simples para integração com sistemas de navegação."

---

## Checkpoint 4 - Avaliação

### Cards Relacionados: 6 e 8

#### Card 6: Métrica de Avaliação

**Pergunta Guia:** Como sabemos que o modelo funciona?

**Diretrizes do Checkpoint 4:**

- **A avaliação começa com perguntas simples, mas poderosas**
- Métricas formais não são obrigatórias, mas são um diferencial

- **Avaliação sem métrica formal (qualitativa):**
  1. **Faz sentido?** O resultado é razoável? Alguém sem conhecimento técnico entenderia?
  2. **Responde o problema?** O output está conectado à pergunta analítica do Checkpoint 1?
  3. **É consistente?** O modelo se comporta de forma previsível e estável?

- **Métricas formais (se disponíveis):**
  - **Precision:** De tudo classificado como positivo, quantos realmente eram?
  - **Recall (Sensibilidade):** De todos os positivos reais, quantos o modelo identificou?
  - **F1-Score:** Média harmônica entre Precision e Recall (útil em classes desbalanceadas)
  - **AUC-ROC:** Capacidade de distinguir classes
  - **RMSE:** Erro quadrático médio (para regressão)
  - **Acurácia Balanceada:** Para classes desbalanceadas

- **O que especificar:**
  - Métrica principal escolhida
  - Por que essa métrica é adequada ao problema de negócio
  - Threshold de aceitação
  - Como será medida

**Exemplo de Resposta:**
"F1-Score como métrica principal. Justificativa: classes desbalanceadas (10% de congestionamentos severos), custo similar de falsos positivos e falsos negativos. Threshold mínimo: 0.75."

#### Card 8: Impacto Esperado

**Pergunta Guia:** Como isso ajudaria na prática?

**Diretrizes do Checkpoint 4:**

- **A avaliação é parte da solução**
- Sem avaliação, não existe entrega, apenas tentativa

- **O que especificar:**
  - Benefícios quantificados (redução de custo, ganho de eficiência, melhora de experiência)
  - Quem se beneficia e como
  - Escala de impacto
  - Comparação com situação atual (baseline)
  - Limitações do modelo (demonstra maturidade técnica)

- **Perguntas para responder:**
  - Qual o impacto econômico esperado?
  - Quais processos serão otimizados?
  - Como a experiência do usuário melhorará?
  - Quais riscos serão reduzidos?

**Exemplo de Resposta:**
"Redução estimada de 25% no tempo de viagem em horários de pico, economia de R$ 500.000/ano em combustível, melhoria na satisfação dos usuários (aumento de 15% no NPS). Limitações: não cobre rotas não mapeadas, dependência de qualidade de dados GPS."

---

## Fluxo de Preenchimento

### Ordem Recomendada

1. **Comece pelo Card 1 (Problema de Negócio)**
   - Defina claramente o problema
   - Se não conseguir explicar em 30 segundos, refine

2. **Passe para o Card 2 (Pergunta Analítica)**
   - Traduza o problema em uma pergunta mensurável
   - Verifique se a pergunta é respondível por IA

3. **Defina o Card 3 (Variável-Alvo)**
   - Especifique o que será previsto
   - Defina tipo e escala

4. **Documente o Card 4 (Dados Disponíveis)**
   - Liste fontes, volume, qualidade
   - Identifique limitações e viéses

5. **Escolha a abordagem no Card 5 (Estratégia de IA)**
   - Justifique a escolha com base nos dados e variável-alvo
   - Seja específico sobre tipo de aprendizado e algoritmos

6. **Planeje o Card 7 (Resultado Esperado)**
   - Defina o escopo do MVP
   - Especifique formato de entrega

7. **Defina a Card 6 (Métrica de Avaliação)**
   - Escolha métrica adequada ao problema
   - Justifique a escolha

8. **Finalize com o Card 8 (Impacto Esperado)**
   - Quantifique benefícios
   - Documente limitações

### Princípios Fundamentais

#### Clareza
- O problema é enunciado com precisão
- Qualquer pessoa do grupo consegue explicar em 30 segundos
- Sem jargões desnecessários

#### Simplicidade
- A solução mais simples que resolve o problema é preferível à mais complexa
- Simplicidade é elegância técnica, não limitação

#### Coerência
- Cada decisão (dados → modelo → métrica → resultado) se sustenta como parte de um raciocínio único
- Não há desconexões entre as seções

---

## Avaliação Prática por Tipo de Desafio

### Desafio 1 - Mobilidade (Inferência de Fluxos)

**Características:**
- Tipo de problema: Aberto, alta ambiguidade
- Foco principal: Inferência e estrutura
- Modelo de referência: Livre escolha
- Principal desafio: Formular a pergunta certa

**Pontos de Atenção:**
- Clareza na definição do problema de negócio
- Precisão na pergunta analítica
- Coerência entre dados disponíveis e abordagem escolhida
- Avaliação qualitativa é essencial (faz sentido o resultado?)

### Desafio 2 - Fire Detection

**Características:**
- Tipo de problema: Estruturado, definido
- Foco principal: Validação e confiabilidade
- Modelo de referência: YOLO (definido)
- Principal desafio: Avaliar falsos positivos

**Pontos de Atenção:**
- Avaliar detecção correta em situações variadas
- Identificar contextos de erro (luz solar, reflexos)
- Documentar falsos positivos e seu impacto
- Métricas de visão computacional (mAP, IoU)

### Desafio 3 - Indústria

**Características:**
- Tipo de problema: Sistema físico real
- Foco principal: Modelagem do processo
- Modelo de referência: Fusão de dados e sensores
- Principal desafio: Integrar múltiplas fontes

**Pontos de Atenção:**
- Integração de diferentes tipos de sensores
- Sincronização temporal de dados
- Robustez em ambientes industriais
- Avaliação em cenários reais de operação

---

## Checklist de Validação

### Antes de Considerar o Canvas Completo

- [ ] **Checkpoint 1:** Problema descrito em uma frase clara?
- [ ] **Checkpoint 1:** Pergunta analítica é específica e mensurável?
- [ ] **Checkpoint 2:** Variável-alvo bem definida (tipo, escala)?
- [ ] **Checkpoint 2:** Dados documentados com limitações?
- [ ] **Checkpoint 2:** Abordagem justificada pelos dados?
- [ ] **Checkpoint 3:** MVP demonstra viabilidade técnica?
- [ ] **Checkpoint 3:** Resultado tem output interpretável?
- [ ] **Checkpoint 4:** Métrica adequada ao problema?
- [ ] **Checkpoint 4:** Avaliação qualitativa realizada (faz sentido, responde, é consistente)?
- [ ] **Checkpoint 4:** Impacto quantificado?
- [ ] **Coerência:** Todas as decisões se conectam logicamente?
- [ ] **Clareza:** Qualquer pessoa do grupo explica em 30 segundos?
- [ ] **Simplicidade:** Solução é a mais simples que resolve o problema?

---

## Erros Comuns a Evitar

### 1. Problema Vago
**Erro:** "Melhorar o trânsito da cidade"
**Correção:** "Reduzir atrasos em 30% nos horários de pico nas 5 principais rotas"

### 2. Pergunta Não Respondível
**Erro:** "O trânsito é bom ou ruim?"
**Correção:** "Qual é a probabilidade de atraso superior a 15 minutos na rota X?"

### 3. Variável-Alvo Indefinida
**Erro:** "Prever trânsito"
**Correção:** "Prever probabilidade de congestionamento (0-1) para cada segmento de rota"

### 4. Dados Não Documentados
**Erro:** "Temos dados de GPS"
**Correção:** "Dados de GPS de 50.000 veículos (2020-2024), atualização a cada 30s, 15% de dados faltantes"

### 5. Abordagem sem Justificativa
**Erro:** "Vamos usar deep learning"
**Correção:** "Random forest devido à interpretabilidade requerida e volume moderado de dados"

### 6. MVP Sem Resultado
**Erro:** "Notebook com código treinando modelo"
**Correção:** "API que retorna previsões para 5 rotas principais"

### 7. Métrica Inadequada
**Erro:** "Acurácia" (para classes desbalanceadas)
**Correção:** "F1-Score" (para classes desbalanceadas)

### 8. Impacto Não Quantificado
**Erro:** "Vai ajudar muito"
**Correção:** "Redução de 25% no tempo de viagem, economia de R$ 500.000/ano"

---

## Responsabilidade do Grupo

### Vocês São Responsáveis pela Solução

Não são apenas executores de código. São responsáveis por:

- **Responsabilidade Técnica:** Entender o que o modelo faz, por que faz e o que significa cada resultado
- **Responsabilidade Analítica:** Questionar os próprios resultados, não aceitar output sem interpretação crítica
- **Responsabilidade de Comunicação:** Apresentar o projeto de forma clara, honesta e convincente

### IA Começa na Forma Como Você Pensa

> "Se você não consegue explicar, você não resolveu o problema."

**Processo:**
1. **Pense primeiro** - Estruture o problema antes de abrir o notebook
2. **Construa com método** - Siga os checkpoints, cada etapa prepara você para a próxima
3. **Saiba explicar** - A explicação é o teste final

---

## Referências

- Material base: "Como Estruturar e Avaliar sua Solução de IA" (slides)
- Canvas de Estrutura do Projeto: `index.html`
- Desafio Final: `biblioteca/1.Desafio-Final-IA-Aplicada-a-Problemas-Reais.pdf`

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:10b981,50:1a56db,100:0f172a&height=120&section=footer" width="100%" alt="Footer"/>
</p>

---
**Resumo:** Guia metodológico completo para preenchimento do Canvas de Estrutura do Projeto de IA, baseado nos 4 checkpoints fundamentais de estruturação e avaliação de soluções de inteligência artificial.
**Data de Criação:** 2026-04-26
**Autor:** Carlos Delfino
**Versão:** 1.0
**Última Atualização:** 2026-04-26
**Atualizado por:** Carlos Delfino
**Histórico de Alterações:**
- 2026-04-26 - Atualizado por Carlos Delfino - Imagens e descrição da Avaliação...
- 2026-04-26 - Criado por Carlos Delfino - Versão 1.0 - Baseado no material "Como Estruturar e Avaliar sua Solução de IA"
