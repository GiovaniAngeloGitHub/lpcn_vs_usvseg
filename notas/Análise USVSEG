# Fluxo do Algoritmo USVSEG

## Visão Geral

O USVSEG é um algoritmo robusto para segmentação de vocalizações ultrassônicas de roedores. O algoritmo processa arquivos de áudio WAV através de uma pipeline de processamento de sinais que identifica e segmenta vocalizações ultrassônicas com base em características espectrais.

## Fluxo Principal do Algoritmo

```mermaid
flowchart TD
    A[Arquivo de Áudio WAV] --> B[Leitura por Blocos]
    B --> C[Espectrograma Multi-Taper]
    C --> D[Achatamento Espectral]
    D --> E[Estimativa de Limiar]
    E --> F[Limiarização]
    F --> G[Detecção Onset/Offset]
    G --> H[Rastreamento de Picos Espectrais]
    H --> I[Segmentação e Saída]
    
    B --> J{Mais blocos?}
    J -->|Sim| C
    J -->|Não| K[Síntese de Som Audível]
    
    I --> L[Arquivos WAV Segmentados]
    I --> M[Imagens Espectrogram]
    I --> N[Traces CSV]
    I --> O[Dados de Análise CSV]
    K --> P[Arquivo de Som Sintético]
    
    style A fill:#e1f5fe
    style C fill:#f3e5f5
    style D fill:#f3e5f5
    style E fill:#fff3e0
    style F fill:#fff3e0
    style G fill:#e8f5e8
    style H fill:#e8f5e8
    style I fill:#fce4ec
```

## Descrição Detalhada dos Passos

### 1. Leitura por Blocos (func.py:674-698)
- Arquivo WAV é lido em blocos de tamanho configurável (padrão: 10s)
- Permite processamento de arquivos grandes sem sobrecarregar memória
- Mantém continuidade entre blocos para detectar vocalizações que cruzam fronteiras

### 2. Espectrograma Multi-Taper (func.py:41-89)
```mermaid
flowchart LR
    A[Sinal de Áudio] --> B[Janelamento Multi-Taper]
    B --> C[FFT]
    C --> D[Magnitude Espectral]
    D --> E[Média dos Tapers]
    E --> F[Conversão para dB]
```
- Utiliza janelas DPSS (Discrete Prolate Spheroidal Sequences)
- Reduz vazamento espectral e melhora estimativa de densidade espectral
- Parâmetros: FFT size=512, time step=0.5ms

### 3. Achatamento Espectral (func.py:91-110)
```mermaid
flowchart LR
    A[Espectrograma] --> B[FFT Cepstral]
    B --> C[Liftering]
    C --> D[IFFT]
    D --> E[Remoção da Mediana]
    E --> F[Filtro Mediano]
```
- Remove variações espectrais de larga escala (formato do trato vocal)
- Enfatiza características espectrais finas (harmônicos, formantes)
- Utiliza análise cepstral com liftering de baixa frequência

### 4. Estimativa de Limiar (func.py:112-126)
- Analisa histograma de amplitudes na banda de frequência de interesse
- Calcula largura à meia altura (FWHM) da distribuição
- Converte para sigma estatístico
- Limiar = sigma × fator de limiar (configurável, padrão: 4.5 SD)

### 5. Limiarização (func.py:128-137)
- Aplica limiar binário ao espectrograma achatado
- Restringe análise à banda de frequência configurada (30-120 kHz padrão)
- Cria máscara binária de atividade vocal

### 6. Detecção de Onset/Offset (func.py:139-225)
```mermaid
flowchart TD
    A[Máscara Binária] --> B[Filtro Temporal]
    B --> C[Detecção de Bordas]
    C --> D[Fusão de Fragmentos]
    D --> E[Filtragem por Gap Mínimo]
    E --> F[Filtragem por Duração]
    F --> G[Adição de Margem]
    G --> H[Fusão de Sobreposições]
```
- Detecta início e fim de vocalizações
- Aplica filtros de duração mínima/máxima e gap mínimo
- Adiciona margens temporais para capturar transientes

### 7. Rastreamento de Picos Espectrais (func.py:366-442)
```mermaid
flowchart TD
    A[Espectrograma Achatado] --> B[Saliência Espectral]
    B --> C[Detecção de Picos]
    C --> D[Centro de Gravidade]
    D --> E[Segregação de Objetos]
    E --> F[Rastreamento Temporal]
    F --> G[Filtragem por Continuidade]
    G --> H[Ordenação por Amplitude]
```
- Identifica picos espectrais dominantes em cada segmento temporal
- Rastreia picos ao longo do tempo para formar trajetórias de frequência
- Calcula características como frequência máxima, média e coeficiente de variação

### 8. Segmentação Sub-syllábica (func.py:502-558)
- Análise morfológica de imagens para detectar componentes espectrais
- Utiliza algoritmo watershed para separar elementos conectados
- Identifica sub-segmentos dentro de vocalizações complexas

## Parâmetros Principais

| Parâmetro | Padrão | Descrição |
|-----------|--------|-----------|
| `fftsize` | 512 | Tamanho da FFT para análise espectral |
| `timestep` | 0.5ms | Passo temporal entre janelas |
| `freqmin` | 30kHz | Frequência mínima de análise |
| `freqmax` | 120kHz | Frequência máxima de análise |
| `threshval` | 4.5 | Fator de limiar em desvios padrão |
| `durmin` | 5ms | Duração mínima de vocalização |
| `durmax` | 300ms | Duração máxima de vocalização |
| `gapmin` | 30ms | Gap mínimo entre vocalizações |
| `margin` | 15ms | Margem temporal adicionada |

## Saídas do Algoritmo

1. **Arquivo CSV de Dados**: Contém onset, offset, duração, frequências e amplitudes
2. **Segmentos WAV**: Vocalizações individuais extraídas
3. **Imagens de Espectrograma**: Visualização dos segmentos detectados
4. **Traces CSV**: Trajetórias de frequência detalhadas
5. **Sub-segmentos CSV**: Componentes espectrais dentro de cada vocalização
6. **Som Sintético**: Versão audível das vocalizações ultrassônicas

## Robustez e Características Especiais

- **Processamento por blocos**: Permite análise de arquivos longos
- **Multi-taper**: Reduz artefatos espectrais
- **Análise cepstral**: Remove variações espectrais grosseiras
- **Rastreamento temporal**: Mantém continuidade de características
- **Filtragem adaptativa**: Ajusta-se às características do sinal
- **Detecção de sub-estrutura**: Identifica componentes espectrais complexos
