# Controle de Paralelização (CPU e GPU)

## Visão Geral

Este documento descreve as configurações de controle de paralelização implementadas no projeto para otimizar o uso de recursos computacionais, limitando o uso de CPU e GPU a 70% por padrão.

## Configurações de Ambiente

### CPU_LIMIT

Define a porcentagem de CPUs que serão usadas para processamento paralelo.

- **Padrão**: `0.7` (70%)
- **Range**: `0.0` a `1.0`
- **Exemplo**: `CPU_LIMIT=0.5` usa 50% das CPUs disponíveis

**Como funciona**:
- Calcula o número de workers Dask baseado no limite configurado
- Configura threads por worker automaticamente
- Aplica a configuração em todas as operações paralelas

### GPU_LIMIT

Define a porcentagem de memória GPU que será usada pelo cuDF/cuML.

- **Padrão**: `0.7` (70%)
- **Range**: `0.0` a `1.0`
- **Exemplo**: `GPU_LIMIT=0.5` usa 50% da memória GPU disponível

**Como funciona**:
- Detecta memória total da GPU
- Calcula limite de memória baseado na porcentagem configurada
- Configura RAPIDS Memory Manager (RMM) com pool de memória limitado
- Aplica o limite em operações cuDF/cuML

## Suporte OpenMP

O projeto configura automaticamente variáveis de ambiente OpenMP para otimizar operações numéricas paralelas:

- **OMP_NUM_THREADS**: Número de threads OpenMP
- **MKL_NUM_THREADS**: Threads para Intel MKL (Math Kernel Library)
- **OPENBLAS_NUM_THREADS**: Threads para OpenBLAS
- **NUMEXPR_NUM_THREADS**: Threads para numexpr

**Como funciona**:
- Calcula automaticamente o número de threads baseado no número de workers Dask
- Configura todas as bibliotecas numéricas para usar o mesmo número de threads
- Evita oversubscription de threads

## Implementação

### Módulos Principais

1. **dask_service.py** (`dynamic_services/dask_service.py`)
   - `get_cpu_limit()`: Retorna limite de CPU configurado
   - `calculate_n_workers()`: Calcula número de workers baseado no limite
   - `configure_dask_cluster()`: Configura cluster Dask com limites e OpenMP
   - `DaskProcessor`: Classe principal com controle de recursos

2. **gpu_detector.py** (`tools/gpu_detector.py`)
   - `get_gpu_limit()`: Retorna limite de GPU configurado
   - `GPUDetector._detect_gpu()`: Detecta GPU e calcula limite de memória
   - `GPUDetector.enable_gpu()`: Habilita GPU com RMM e limite de memória

### Configuração Dask

O cluster Dask é configurado com:

```python
LocalCluster(
    n_workers=n_workers,
    threads_per_worker=threads_per_worker,
    memory_limit='auto',
    processes=True,
    silence_logs=True,
    dashboard_address=None
)
```

Configurações de memória:
- `distributed.worker.memory.target`: 60%
- `distributed.worker.memory.spill`: 70%
- `distributed.worker.memory.pause`: 80%
- `distributed.worker.memory.terminate`: 95%

### Configuração GPU

O limite de memória GPU é configurado usando RAPIDS Memory Manager:

```python
import rmm
import rmm.mr

mr = rmm.mr.CudaMemoryResource()
pool_mr = rmm.mr.PoolMemoryResource(
    mr, 
    initial_pool_size=gpu_memory_limit // 2,
    maximum_pool_size=gpu_memory_limit
)
rmm.mr.set_current_device_resource(pool_mr)
```

## Testes

Testes unitários estão disponíveis em `tests/dynamic_services/test_parallelization.py`:

- `TestCPUControl`: Testes para controle de CPU
- `TestGPUControl`: Testes para controle de GPU
- `TestDaskProcessor`: Testes para DaskProcessor
- `TestGPUDetector`: Testes para GPUDetector
- `TestOpenMPConfiguration`: Testes para configuração OpenMP

**Executar testes**:
```bash
pytest tests/dynamic_services/test_parallelization.py -v
```

## Exemplos de Uso

### Configuração Padrão (70%)

```python
from dynamic_services.dask_service import DaskProcessor
from tools.gpu_detector import GPUDetector

# Dask com controle de CPU
processor = DaskProcessor()
print(f"Workers: {processor.n_workers}")
print(f"Threads per worker: {processor.threads_per_worker}")

# GPU com controle de memória
detector = GPUDetector()
detector.enable_gpu()
print(f"GPU limit: {detector.gpu_limit}")
print(f"GPU memory limit: {detector.gpu_memory_limit / 1024**3:.2f} GB")
```

### Configuração Customizada

```bash
# No arquivo .env
CPU_LIMIT=0.5
GPU_LIMIT=0.5
```

```python
# O código automaticamente usa os novos limites
processor = DaskProcessor()  # Usará 50% da CPU
detector = GPUDetector()  # Usará 50% da memória GPU
```

## Benefícios

1. **Prevenção de Oversubscription**: Evita sobrecarga de recursos do sistema
2. **Estabilidade**: Mantém recursos disponíveis para outros processos
3. **Performance Otimizada**: Balanceia paralelismo com uso de recursos
4. **Configuração Flexível**: Permite ajuste via variáveis de ambiente
5. **Logging Estruturado**: Monitoramento detalhado de uso de recursos

## Dependências

- `dask[complete]>=2024.0.0`: Processamento paralelo
- `pynvml>=11.0.0`: Detecção de GPU NVIDIA
- `numexpr>=2.8.0`: Aceleração de expressões numéricas com OpenMP
- `rmm>=24.0.0` (opcional): RAPIDS Memory Manager para controle de memória GPU
- `cudf>=24.0.0` (opcional): DataFrames acelerados por GPU
- `cuml>=24.0.0` (opcional): Machine learning acelerado por GPU

## Troubleshooting

### CPU não está sendo limitada

Verifique se a variável de ambiente `CPU_LIMIT` está configurada:
```python
import os
print(os.getenv('CPU_LIMIT', '0.7'))
```

### GPU não está sendo limitada

Verifique se a variável de ambiente `GPU_LIMIT` está configurada:
```python
import os
print(os.getenv('GPU_LIMIT', '0.7'))
```

### OpenMP não está funcionando

Verifique se as variáveis de ambiente OpenMP estão configuradas:
```python
import os
print(f"OMP_NUM_THREADS: {os.getenv('OMP_NUM_THREADS')}")
print(f"MKL_NUM_THREADS: {os.getenv('MKL_NUM_THREADS')}")
```

### Erro ao configurar RMM

Se o RMM não estiver disponível, o sistema continuará sem limite de memória GPU:
```
⚠️  Erro ao configurar limite de memória GPU: ...
✓ GPU habilitada usando cuDF/cuML (sem limite de memória)
```

## Referências

- [Dask Documentation](https://docs.dask.org/)
- [RAPIDS Documentation](https://rapids.ai/)
- [OpenMP Documentation](https://www.openmp.org/)
- [RMM Documentation](https://docs.rapids.ai/api/rmm/stable/)

---
**Resumo:** Documentação completa do sistema de controle de paralelização com limites de CPU (70%) e GPU (70%), suporte OpenMP e configuração de Dask LocalCluster.
**Data de Criação:** 2026-05-02
**Autor:** Rapport GenerAtiva
**Versão:** 1.0
**Última Atualização:** 2026-05-08
**Atualizado por:** Carlos Delfino
**Histórico de Alterações:**
- 2026-05-08 - Atualizado por Carlos Delfino - Diversos ajustes e novos arquivos....
- 2026-05-02 - Criado por Rapport GenerAtiva - Versão 1.0
