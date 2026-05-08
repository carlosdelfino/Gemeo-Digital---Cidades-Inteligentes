![visitors](https://visitor-badge.laobi.icu/badge?page_id=carlosdelfino.Gemeo-Digital---Cidades-Inteligentes)
[![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC_BY--SA_4.0-blue.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
![Language: Portuguese](https://img.shields.io/badge/Language-Portuguese-brightgreen.svg)
![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Dask](https://img.shields.io/badge/Dask-Distributed-orange)
![Machine Learning](https://img.shields.io/badge/Machine%20Learning-Prática-green)
![Status](https://img.shields.io/badge/Status-Educa%C3%A7%C3%A3o-brightgreen)
![Repository Size](https://img.shields.io/github/repo-size/carlosdelfino/Gemeo-Digital---Cidades-Inteligentes)
![Last Commit](https://img.shields.io/github/last-commit/carlosdelfino/Gemeo-Digital---Cidades-Inteligentes)

<!-- Animated Header -->
<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f172a,50:1a56db,100:10b981&height=220&section=header&text=Computa%C3%A7%C3%A3o%20Distribu%C3%ADda%20Dask&fontSize=42&fontColor=ffffff&animation=fadeIn&fontAlignY=35&desc=Tutorial%20de%20Configura%C3%A7%C3%A3o%20de%20Cluster%20Distribu%C3%ADdo&descSize=18&descAlignY=55&descColor=94a3b8" width="100%" alt="Computação Distribuída Dask Header"/>
</p>

## Tutorial: Computação Distribuída com Dask

### Visão Geral

Este tutorial explica como configurar computação distribuída usando Dask no projeto Statistical and Analytical Agentic, permitindo distribuir o processamento entre múltiplos nós (máquinas) para aumentar a capacidade computacional.

### Arquitetura Atual vs Distribuída

#### Arquitetura Atual (LocalCluster)

O projeto atual utiliza `LocalCluster` do Dask, que executa todos os workers na mesma máquina:

```python
from dask.distributed import Client, LocalCluster

cluster = LocalCluster(
    n_workers=n_workers,
    threads_per_worker=threads_per_worker,
    memory_limit='auto',
    processes=True,
    silence_logs=True,
    dashboard_address=None
)
client = Client(cluster)
```

**Limitações:**
- Processamento limitado aos recursos de uma única máquina
- Escalabilidade restrita ao hardware disponível
- Não aproveita recursos de múltiplas máquinas

#### Arquitetura Distribuída (Distributed Cluster)

Na arquitetura distribuída, temos:
- **Scheduler**: Coordena a distribuição de tarefas
- **Workers**: Executam as tarefas em máquinas diferentes
- **Client**: Submete tarefas ao scheduler

```
┌─────────────┐         ┌─────────────┐         ┌─────────────┐
│   Client    │────────▶│  Scheduler  │◀────────│   Worker 1  │
│ (Máquina A) │         │ (Máquina A) │         │ (Máquina B) │
└─────────────┘         └─────────────┘         └─────────────┘
                                │
                                ▼
                        ┌─────────────┐
                        │   Worker 2  │
                        │ (Máquina C) │
                        └─────────────┘
```

### Pré-requisitos

#### Requisitos de Rede

- **Conectividade**: Todas as máquinas devem conseguir se comunicar via rede
- **Portas**: Porta 8786 (padrão do scheduler Dask) deve estar aberta
- **SSH**: Acesso SSH entre máquinas (recomendado para segurança)
- **Firewall**: Configure firewall para permitir conexões na porta do scheduler

#### Requisitos de Software

Todas as máquinas devem ter:
- Python 3.8+
- Dask instalado: `pip install dask[complete]`
- Dependências do projeto instaladas
- Variáveis de ambiente configuradas (CPU_LIMIT, GPU_LIMIT)

#### Requisitos de Hardware

- **Máquina do Scheduler**: CPU mínima de 4 cores, 8GB RAM
- **Máquinas dos Workers**: CPU mínima de 2 cores, 4GB RAM cada
- **Armazenamento**: Acesso compartilhado aos dados (NFS, S3, ou cópia local)

### Configuração do Scheduler

#### Passo 1: Preparar a Máquina do Scheduler

Na máquina que executará o scheduler (Máquina A):

```bash
# Clone o repositório
git clone <repo-url>
cd statistical-and-analytical-agentic

# Crie ambiente virtual
python -m venv venv
source venv/bin/activate  # Linux/Mac
# venv\Scripts\activate  # Windows

# Instale dependências
pip install -r requirements.txt
pip install dask[complete]

# Configure variáveis de ambiente
export CPU_LIMIT=0.7
export GPU_LIMIT=0.7
```

#### Passo 2: Iniciar o Scheduler

Crie um script `start_scheduler.py`:

```python
#!/usr/bin/env python
"""
Script para iniciar o scheduler Dask em modo distribuído
"""
import os
import sys
from pathlib import Path
from dask.distributed import Scheduler
import logging

# Configura logging
logging.basicConfig(level=logging.INFO)

# Configura limites de CPU
cpu_limit = float(os.getenv('CPU_LIMIT', '0.7'))
n_workers_scheduler = max(1, int(os.cpu_count() * cpu_limit))

# Inicia scheduler
scheduler = Scheduler(
    host='0.0.0.0',  # Escuta em todas as interfaces
    port=8786,       # Porta padrão do Dask
    dashboard_address=':8787'  # Dashboard na porta 8787
)

print(f"✅ Scheduler iniciado em: {scheduler.address}")
print(f"📊 Dashboard disponível em: http://<IP_DA_MAQUINA>:8787")
print(f"🔧 CPU Limit: {cpu_limit}")
print(f"👥 Workers do scheduler: {n_workers_scheduler}")

# Mantém o scheduler rodando
try:
    scheduler.run_forever()
except KeyboardInterrupt:
    print("\n🛑 Scheduler encerrado")
    scheduler.close()
```

Execute o scheduler:

```bash
python start_scheduler.py
```

O scheduler mostrará algo como:
```
✅ Scheduler iniciado em: tcp://192.168.1.100:8786
📊 Dashboard disponível em: http://192.168.1.100:8787
```

**Anote o endereço do scheduler** (ex: `tcp://192.168.1.100:8786`) - você precisará dele para configurar os workers.

### Configuração dos Workers

#### Passo 1: Preparar Cada Máquina Worker

Repita em cada máquina worker (Máquina B, Máquina C, etc.):

```bash
# Clone o repositório
git clone <repo-url>
cd statistical-and-analytical-agentic

# Crie ambiente virtual
python -m venv venv
source venv/bin/activate  # Linux/Mac
# venv\Scripts\activate  # Windows

# Instale dependências
pip install -r requirements.txt
pip install dask[complete]

# Configure variáveis de ambiente
export CPU_LIMIT=0.7
export GPU_LIMIT=0.7
```

#### Passo 2: Iniciar Workers

Crie um script `start_worker.py` em cada worker:

```python
#!/usr/bin/env python
"""
Script para iniciar um worker Dask conectado ao scheduler distribuído
"""
import os
import sys
from pathlib import Path
from dask.distributed import Client, Worker
import multiprocessing
import logging

# Configura logging
logging.basicConfig(level=logging.INFO)

# Endereço do scheduler (altere para o endereço real)
SCHEDULER_ADDRESS = os.getenv('SCHEDULER_ADDRESS', 'tcp://192.168.1.100:8786')

# Configura limites de CPU
cpu_limit = float(os.getenv('CPU_LIMIT', '0.7'))
n_workers = max(1, int(os.cpu_count() * cpu_limit))
threads_per_worker = max(1, int(multiprocessing.cpu_count() / n_workers))

print(f"🔗 Conectando ao scheduler: {SCHEDULER_ADDRESS}")
print(f"🔧 CPU Limit: {cpu_limit}")
print(f"👥 Número de workers: {n_workers}")
print(f"🧵 Threads por worker: {threads_per_worker}")

# Cria worker
worker = Worker(
    scheduler_address=SCHEDULER_ADDRESS,
    nthreads=threads_per_worker,
    memory_limit='auto',
    dashboard_address=None
)

print(f"✅ Worker iniciado e conectado ao scheduler")

# Mantém o worker rodando
try:
    worker.run_forever()
except KeyboardInterrupt:
    print("\n🛑 Worker encerrado")
    worker.close()
```

Execute o worker em cada máquina:

```bash
# Máquina B
export SCHEDULER_ADDRESS=tcp://192.168.1.100:8786
python start_worker.py

# Máquina C
export SCHEDULER_ADDRESS=tcp://192.168.1.100:8786
python start_worker.py
```

#### Opção Alternativa: Usar dask-worker CLI

O Dask também fornece uma CLI para iniciar workers:

```bash
# Em cada máquina worker
dask-worker tcp://192.168.1.100:8786 \
  --nthreads 4 \
  --memory-limit 4GB \
  --name worker-$(hostname)
```

### Adaptando o Código do Projeto

#### Modificar `dask_service.py`

O arquivo `dynamic_services/dask_service.py` precisa ser modificado para suportar cluster distribuído. Adicione uma nova função:

```python
def configure_distributed_cluster(scheduler_address: Optional[str] = None):
    """
    Configura o cluster Dask em modo distribuído
    
    Args:
        scheduler_address: Endereço do scheduler (ex: 'tcp://192.168.1.100:8786')
                          Se None, usa LocalCluster (modo padrão)
    
    Returns:
        tuple: (n_workers, threads_per_worker, client, cluster/scheduler)
    """
    n_workers = calculate_n_workers()
    threads_per_worker = max(1, int(multiprocessing.cpu_count() / n_workers))
    
    # Configurações Dask
    dask.config.set({
        'distributed.worker.memory.target': '0.6',
        'distributed.worker.memory.spill': '0.7',
        'distributed.worker.memory.pause': '0.8',
        'distributed.worker.memory.terminate': '0.95',
    })
    
    # Configura OpenMP
    os.environ['OMP_NUM_THREADS'] = str(threads_per_worker)
    os.environ['MKL_NUM_THREADS'] = str(threads_per_worker)
    os.environ['OPENBLAS_NUM_THREADS'] = str(threads_per_worker)
    os.environ['NUMEXPR_NUM_THREADS'] = str(threads_per_worker)
    
    # Verifica se deve usar cluster distribuído
    use_distributed = scheduler_address or os.getenv('DASK_SCHEDULER_ADDRESS')
    
    if use_distributed:
        # Modo distribuído
        addr = scheduler_address or os.getenv('DASK_SCHEDULER_ADDRESS')
        try:
            client = Client(addr)
            
            log_event('SUCCESS', 'Cluster Dask configurado em modo distribuído',
                       scheduler_address=addr,
                       n_workers=len(client.scheduler_info()['workers']),
                       cpu_limit=get_cpu_limit(),
                       openmp_threads=threads_per_worker)
            
            return n_workers, threads_per_worker, client, addr
        except Exception as e:
            log_event('ERROR', 'Erro ao conectar ao scheduler distribuído', error=str(e))
            log_event('INFO', 'Fallback para LocalCluster')
            # Fallback para LocalCluster
            return configure_dask_cluster_local()
    else:
        # Modo local (padrão)
        return configure_dask_cluster_local()


def configure_dask_cluster_local():
    """
    Configura o cluster Dask local (LocalCluster)
    Função auxiliar para fallback
    """
    n_workers = calculate_n_workers()
    threads_per_worker = max(1, int(multiprocessing.cpu_count() / n_workers))
    
    try:
        cluster = LocalCluster(
            n_workers=n_workers,
            threads_per_worker=threads_per_worker,
            memory_limit='auto',
            processes=True,
            silence_logs=True,
            dashboard_address=None
        )
        
        client = Client(cluster)
        
        log_event('SUCCESS', 'Cluster Dask configurado com LocalCluster',
                   n_workers=n_workers,
                   threads_per_worker=threads_per_worker,
                   cpu_limit=get_cpu_limit(),
                   openmp_threads=threads_per_worker,
                   cluster_address=str(cluster.scheduler_address))
        
        return n_workers, threads_per_worker, client, cluster
    except Exception as e:
        log_event('ERROR', 'Erro ao criar LocalCluster', error=str(e))
        return n_workers, threads_per_worker, None, None
```

#### Modificar a classe `DaskProcessor`

Atualize o método `_configure_cluster`:

```python
def _configure_cluster(self):
    """Configura o cluster Dask com limites de CPU e OpenMP"""
    try:
        # Tenta configurar cluster distribuído se endereço fornecido
        scheduler_address = os.getenv('DASK_SCHEDULER_ADDRESS')
        self.n_workers, self.threads_per_worker, self.client, self.cluster = \
            configure_distributed_cluster(scheduler_address)
    except Exception as e:
        log_event('ERROR', 'Erro ao configurar cluster Dask', error=str(e))
        # Fallback para valores seguros
        self.n_workers = max(1, multiprocessing.cpu_count() // 2)
        self.threads_per_worker = 1
```

### Configuração de Ambiente

#### Arquivo `.env`

Adicione ao arquivo `.env` na máquina cliente:

```bash
# Configuração de cluster distribuído
DASK_SCHEDULER_ADDRESS=tcp://192.168.1.100:8786

# Limites de CPU/GPU (usados em cada worker)
CPU_LIMIT=0.7
GPU_LIMIT=0.7
```

#### Variáveis de Ambiente Opcionais

```bash
# Número de workers específico (sobrescreve cálculo automático)
DASK_N_WORKERS=4

# Memória por worker
DASK_MEMORY_LIMIT=4GB

# Dashboard do scheduler
DASK_DASHBOARD_ADDRESS=:8787
```

### Exemplos de Uso

#### Exemplo 1: Usar Cluster Distribuído

```python
from dynamic_services.dask_service import DaskProcessor

# O cluster distribuído é configurado automaticamente
# se DASK_SCHEDULER_ADDRESS estiver definido
processor = DaskProcessor()

# Carrega dataset
result = processor.load_csv_with_dask(
    '/path/to/dataset.csv',
    blocksize='256MB'
)

# Computa estatísticas (executado em paralelo nos workers)
stats = processor.compute_statistics()
```

#### Exemplo 2: Conectar Manualmente ao Scheduler

```python
from dask.distributed import Client
import dask.dataframe as dd

# Conecta ao scheduler distribuído
client = Client('tcp://192.168.1.100:8786')

# Verifica workers conectados
print(f"Workers conectados: {len(client.scheduler_info()['workers'])}")

# Carrega dataset com Dask
ddf = dd.read_csv('/path/to/dataset.csv', blocksize='256MB')

# Computa estatísticas
stats = ddf.describe().compute()

client.close()
```

#### Exemplo 3: Monitorar com Dashboard

Acesse o dashboard do scheduler:

```
http://192.168.1.100:8787
```

O dashboard mostra:
- Status dos workers
- Tarefas em execução
- Uso de memória e CPU
- Tempo de execução das tarefas

### Gerenciamento de Dados

#### Opção 1: Armazenamento Compartilhado (NFS)

Configure NFS para que todas as máquinas acessem os mesmos dados:

```bash
# No servidor NFS (Máquina A)
sudo apt-get install nfs-kernel-server
sudo mkdir /shared_data
sudo chmod 777 /shared_data
sudo exportfs -o rw,sync,no_subtree_check /shared_data *(rw,no_subtree_check)

# Nos clientes (Máquinas B, C, etc.)
sudo apt-get install nfs-common
sudo mount 192.168.1.100:/shared_data /mnt/shared_data
```

#### Opção 2: Cópia Local de Dados

Copie os dados para cada máquina:

```bash
# Em cada worker
scp -r user@192.168.1.100:/path/to/dataset /local/path/
```

#### Opção 3: Armazenamento em Nuvem (S3)

Use S3 ou outro armazenamento em nuvem:

```python
import dask.dataframe as dd

# Lê dados do S3
ddf = dd.read_parquet('s3://bucket/dataset/*.parquet',
                     storage_options={'key': 'AWS_KEY', 
                                    'secret': 'AWS_SECRET'})
```

### Troubleshooting

#### Workers não conectam ao scheduler

**Solução:**
1. Verifique conectividade de rede: `ping <scheduler_ip>`
2. Verifique se a porta 8786 está aberta no firewall
3. Verifique logs do scheduler para erros
4. Confirme que o endereço do scheduler está correto

```bash
# Testar conexão
telnet 192.168.1.100 8786

# Verificar firewall
sudo ufw status
sudo ufw allow 8786
```

#### Erro de timeout ao conectar

**Solução:**
Aumente o timeout no cliente:

```python
from dask.distributed import Client

client = Client('tcp://192.168.1.100:8786',
                timeout='60s')
```

#### Workers sem memória suficiente

**Solução:**
Ajuste o limite de memória por worker:

```bash
# No script do worker
dask-worker tcp://192.168.1.100:8786 --memory-limit 8GB
```

Ou configure no `.env`:
```bash
DASK_MEMORY_LIMIT=8GB
```

#### Dados não acessíveis nos workers

**Solução:**
1. Verifique se o caminho dos dados é o mesmo em todas as máquinas
2. Use caminhos absolutos ou compartilhados
3. Verifique permissões de arquivo

```python
# Verifique se o arquivo existe no worker
import os
print(os.path.exists('/path/to/dataset.csv'))
```

### Boas Práticas

#### 1. Segurança

- Use SSH para comunicação entre máquinas
- Configure firewall para permitir apenas IPs confiáveis
- Use autenticação no scheduler se necessário
- Não exponha o dashboard publicamente

#### 2. Monitoramento

- Monitore o dashboard do scheduler regularmente
- Configure logs estruturados em todas as máquinas
- Use alertas para falhas de workers
- Monitore uso de recursos (CPU, memória, rede)

#### 3. Escalabilidade

- Adicione workers dinamicamente conforme necessidade
- Use auto-scaling com Kubernetes (avançado)
- Balanceie carga entre workers
- Otimize particionamento dos dados

#### 4. Resiliência

- Configure retry automático em falhas
- Use checkpoints para tarefas longas
- Mantenha backup dos dados
- Documente configurações de rede

### Testando a Configuração

#### Teste 1: Verificar Conexão

```python
from dask.distributed import Client

# Conecta ao scheduler
client = Client('tcp://192.168.1.100:8786')

# Verifica workers
print(f"Workers: {len(client.scheduler_info()['workers'])}")

# Testa computação simples
def inc(x):
    return x + 1

futures = client.map(inc, range(10))
results = client.gather(futures)
print(f"Resultados: {results}")

client.close()
```

#### Teste 2: Processamento de Dados

```python
from dynamic_services.dask_service import DaskProcessor

processor = DaskProcessor()

# Carrega dataset
result = processor.load_csv_with_dask('/path/to/test.csv')
print(f"Partições: {result['result']['npartitions']}")

# Computa estatísticas
stats = processor.compute_statistics()
print(f"Estatísticas: {stats['success']}")
```

#### Teste 3: Escalabilidade

```python
from dask.distributed import Client
import time

client = Client('tcp://192.168.1.100:8786')

# Testa com diferentes tamanhos de workload
for n in [100, 1000, 10000]:
    start = time.time()
    futures = client.map(lambda x: x**2, range(n))
    results = client.gather(futures)
    elapsed = time.time() - start
    print(f"N={n}, Tempo={elapsed:.2f}s")

client.close()
```

### Referências

- [Dask Distributed Documentation](https://docs.dask.org/en/stable/deploying.html)
- [Dask Scheduler Configuration](https://docs.dask.org/en/stable/scheduling.html)
- [Dask Worker Configuration](https://docs.dask.org/en/stable/setup.html)
- [Dask Dashboard](https://docs.dask.org/en/stable/dashboard.html)

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:10b981,50:1a56db,100:0f172a&height=120&section=footer" width="100%" alt="Footer"/>
</p>

---
**Resumo:** Tutorial completo para configurar computação distribuída com Dask, incluindo setup de scheduler, workers, adaptação do código do projeto, gerenciamento de dados e troubleshooting.
**Data de Criação:** 2026-05-03
**Autor:** Rapport GenerAtiva
**Versão:** 1.0
**Última Atualização:** 2026-05-08
**Atualizado por:** Carlos Delfino
**Histórico de Alterações:**
- 2026-05-08 - Atualizado por Carlos Delfino - Diversos ajustes e novos arquivos....
- 2026-05-03 - Criado por Rapport GenerAtiva - Versão 1.0
