![visitors](https://visitor-badge.laobi.icu/badge?page_id=ArvoreDosSaberes.Gemeo-Digital---Cidades-Inteligentes)
[![License: Private](https://img.shields.io/badge/License-Private-red.svg)](mailto:consultoria@carlosdelfino.eti.br)
![Language: Portuguese](https://img.shields.io/badge/Language-Portuguese-brightgreen.svg)
![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange)
![Machine Learning](https://img.shields.io/badge/Machine%20Learning-Prática-green)
![Status](https://img.shields.io/badge/Status-Educa%C3%A7%C3%A3o-brightgreen)
![Repository Size](https://img.shields.io/github/repo-size/ArvoreDosSaberes/Gemeo-Digital---Cidades-Inteligentes)
![Last Commit](https://img.shields.io/github/last-commit/ArvoreDosSaberes/Gemeo-Digital---Cidades-Inteligentes)

<!-- Animated Header -->
<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f172a,50:1a56db,100:10b981&height=220&section=header&text=Cidades%20Inteligentes&fontSize=42&fontColor=ffffff&animation=fadeIn&fontAlignY=35&desc=IA%20Aplicada%20a%20Problemas%20Reais&descSize=18&descAlignY=55&descColor=94a3b8" width="100%" alt="Cidades Inteligentes Header"/>
</p>

## Descrição do Projeto

Este projeto é parte do desafio de IA aplicada a problemas reais, focado em cidades inteligentes. O objetivo é desenvolver soluções de inteligência artificial para inferência de fluxos origem-destino em ambientes urbanos.

**Desafio Final:** IA Aplicada a Problemas Reais (Mobilidade Urbana)  
**Residência em Gêmeo Digital em 5G** — Facens

### 👥 A Equipe: Pearsonianos (Desafio 1 Splice)
*   **Binha Ferraz Dauma** | **Ednardo Pinheiro Peixoto** | **Eric Pimentel** | **Luis Felipe Ferreira**
*   **Carlos Delfino** | **Dennis Giancarlo** | **Ana Temoteo** | **Adriano José**

### Objetivos

Gestores públicos de mobilidade necessitam de respostas baseadas em dados para alocação de infraestrutura. O nosso desafio técnico foi **inferir padrões de fluxo de Origem-Destino (O-D)** na cidade de Sorocaba-SP, utilizando uma malha de sensores de tráfego que, originalmente, **não fornecia rótulos explícitos de viagem**.

Nosso foco foi transformar 76 milhões de registros (pings) desconexos em **Inteligência Acionável** capaz de alimentar as premissas de um Gêmeo Digital Urbano.

### Objetivos Técnicos

- Desenvolver modelos de IA para análise de mobilidade urbana
- Inferir padrões de fluxo origem-destino
- Aplicar técnicas de machine learning em dados de cidades inteligentes
- Estruturar e avaliar soluções de IA seguindo melhores práticas

### Estrutura do Projeto

```text
projeto/
├── data/                 # (Baixar do Drive) Bases divididas em Raw, Bronze, Silver e Gold
├── docs/                 # Documentação técnica, Canvas PDF e artefatos executivos
├── scripts/              # Scripts Python para processamento e análise
│   ├── audit_data.py                  # Profiling e Data Quality
│   ├── clean_bronze.py                # Higienização e tipagem (Camada Bronze)
│   ├── trip_segmentation.py           # Window Functions para Matriz O-D (Camada Silver)
│   ├── spatial_clustering_ia.py       # Modelo DBSCAN Não-Supervisionado (Camada Gold)
│   ├── generate_temporal_analysis.py  # Dashboard interativo Plotly
│   ├── generate_map.py                # Mapa Geoespacial Folium (Gêmeo Digital)
│   ├── markdown_history_manager.py    # Sistema de histórico automático
│   └── pre-commit-hook-example        # Exemplo de hook git
├── notebooks/            # Experimentações em Jupyter
├── biblioteca/           # Documentos de referência e materiais didáticos
├── venv/                 # Ambiente virtual Python
├── index.html            # Portfólio Web Interativo (MVP Executivo)
└── README.md             # Documentação principal
```

### 💾 Acesso à Base de Dados (Google Drive)

Os dados massivos originais não estão hospedados neste repositório devido ao seu volume (> 5GB). Toda a base de dados que compõe as pastas `data/00_raw` até `data/03_gold` encontra-se [**Disponível neste Link do Google Drive**](https://drive.google.com/drive/folders/1Gw9GFrNwx6wabkjHKkRqk0U1aJmJ-wy5?usp=drive_link). Para executar os códigos localmente, faça o download do conteúdo do link e coloque-o na raiz do projeto dentro da pasta `data/`.

### Metodologia

O projeto segue a metodologia PDCL (Plan, Do, Check, Logs):

- **Plan**: Planejamento de funcionalidades, requisitos e arquitetura
- **Do**: Implementação seguindo o planejamento com código limpo e logs estruturados
- **Check**: Execução de testes completos verificando requisitos e analisando logs
- **Logs**: Registros detalhados de todas as atividades e decisões

### Como Começar

**1. Instalação e Preparação:**

```bash
# Clone este repositório
git clone https://github.com/SEU-USUARIO/desafio1.git
cd desafio1

# Crie e ative um ambiente virtual
python -m venv .venv
source .venv/bin/activate  # No Linux/Mac
.venv\Scripts\activate     # No Windows

# Instale as bibliotecas principais
pip install duckdb pandas scikit-learn plotly folium numpy
```

*Lembrete: Baixe a pasta `data/` do Google Drive e insira na raiz antes de prosseguir.*

**2. Ordem de Execução do Pipeline:**

```bash
# 1. Auditoria e Limpeza (Opcional se já baixar os parquets prontos)
python scripts/audit_data.py
python scripts/clean_bronze.py

# 2. Extração da Matriz Origem-Destino
python scripts/trip_segmentation.py

# 3. Modelagem IA Não-Supervisionada
python scripts/spatial_clustering_ia.py

# 4. Geração do Gêmeo Digital Visuais
python scripts/generate_temporal_analysis.py
python scripts/generate_map.py
```

### ⚙️ Engenharia de Dados (Pipeline ETL)

Construímos uma arquitetura de dados escalável para processar volumes massivos sem estouro de memória (RAM), utilizando processamento *Out-Of-Core* via **DuckDB** e compressão colunar **Parquet**.

O pipeline flui estritamente por 4 camadas:

1. **Camada Raw (`00_raw`):** Os arquivos CSV pesados originados da captura de IoT (Sensores).
2. **Camada Bronze (`01_bronze` - *clean_bronze.py*):** Ingestão bruta, higienização de separadores decimais falhos (vírgulas vs pontos) e conversão de strings de datas para objetos matemáticos de tempo (`TIMESTAMP`).
3. **Camada Silver (`02_silver` - *trip_segmentation.py*):** Onde a mágica analítica acontece. Aplicação de *Window Functions* para criar uma **Janela de Inatividade de 45 minutos**. Se um carro desaparece dos radares por mais de 45 minutos, declaramos sua rota como finalizada matematicamente. Aqui nasce a primeira *Matriz Origem-Destino Bruta*.
4. **Camada Gold (`03_gold` - *spatial_clustering_ia.py*):** A Inteligência Artificial em ação. O algoritmo geoespacial **DBSCAN** consome os pares O-D da camada Silver para agrupar as coordenadas físicas em pólos de alta densidade veicular. A IA formou **3 Macro-Zonas**, obtendo um *Silhouette Score* de `0.3134` (alta coesão espacial). O output final é a matriz de Macro-Corredores pronta para BI.

### Documentação

Para mais detalhes sobre a metodologia e regras do projeto, consulte:
- `.windsurf/rules/documentacao.md` - Regras de formatação markdown
- `.windsurf/rules/logs.md` - Sistema de logging estruturado
- `.windsurf/rules/projeto.md` - Metodologia PDCL
- `.windsurf/rules/validacao_da_das_correcoes_e_testes.md` - Validação e testes

### Sistema de Histórico Automático

O projeto possui um hook de pre-commit que atualiza automaticamente o histórico de alterações em arquivos markdown. Este sistema é gerenciado pelo script `scripts/markdown_history_manager.py`.

### 🌐 Portfólio Interativo (MVP Web)

Como coroamento do projeto, consolidamos os dados em uma página executiva otimizada para o **GitHub Pages**. O site de portfólio (localizado no `index.html` do repositório) foi desenhado com estética *Dark/Cyber* para materializar o conceito de "Smart City". Nele, disponibilizamos:

*   As regras do nosso **Business Model Canvas** em cards interativos.
*   **Dashboard Temporal:** Gráficos interativos renderizados em Plotly.
*   **Mapa Interativo de Fluxo:** Mapa tático Folium desenhando fisicamente a animação de tráfego pesado operando entre os centros de massa (Macro-Zonas) definidos pela Inteligência Artificial.

### 🚧 Desafios Encontrados e Soluções Adotadas

*   **Problema (Estouro de Memória):** Notebooks congelando ao tentar ler a base raw (12GB csv).
    *Solução:* Adoção da arquitetura *DuckDB + Parquet* em streaming.
*   **Problema (Ausência de Target/Variável Alvo):** Nenhuma coluna dizia para onde os carros iam.
    *Solução:* Desenvolvimento de uma heurística espaço-temporal de 45 minutos para "recortar" o histórico do veículo em sub-viagens.
*   **Problema (Divergência Documental):** O edital falava em 50 sensores, mas a base continha lixo e radares anômalos.
    *Solução:* Script rigoroso de *Data Profiling* para atestar os 61 equipamentos físicos reais e remover hardwares "fantasmas".

### 🚀 Melhorias Futuras e Roadmap

1.  **Integração em Tempo Real (Kafka/Streaming):** Evoluir o pipeline em lote (*batch*) para ingestão de dados por telemetria contínua.
2.  **Machine Learning Preditivo (Deep Learning):** Alimentar Redes Neurais Long Short-Term Memory (LSTM) com o resultado das nossas Macro-Zonas para não apenas ler o presente, mas **prever engarrafamentos** em janelas de 30 minutos no futuro.
3.  **Fusão de Dados (Clima e Eventos):** Enriquecer a matriz com dados meteorológicos e agenda da cidade (shows, feriados) para criar um Gêmeo Digital responsivo ao ambiente urbano vivo.

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:10b981,50:1a56db,100:0f172a&height=120&section=footer" width="100%" alt="Footer"/>
</p>

---
**Resumo:** Projeto de IA aplicada a cidades inteligentes focado em inferência de fluxos origem-destino em ambientes urbanos.
**Data de Criação:** 2025-04-25
**Autor:** Rapport GenerAtiva
**Versão:** 1.0
**Última Atualização:** 2026-05-08
**Atualizado por:** Carlos Delfino
**Histórico de Alterações:**
- 2026-05-08 - Atualizado por Carlos Delfino - Diversos ajustes e novos arquivos....
- 2026-05-01 - Atualizado por Carlos Delfino - Canva para orientar nosso projeto...
- 2026-04-29 - Atualizado por Carlos Delfino - novos ajustes...
- 2026-04-28 - Atualizado por Carlos Delfino - Scripts para hooks do git....
- 2026-04-25 - Atualizado por Carlos Delfino - Slides e informação sobre o projeto....
- 2026-04-25 - Atualizado por Carlos Delfino - Slides e informação sobre o projeto....
- 2025-04-25 - Criado por Rapport GenerAtiva - Versão 1.0
