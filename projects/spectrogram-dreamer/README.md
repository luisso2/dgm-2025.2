# `Análise de Sinais de Áudio com Modelos de Mundo`
# `Audio Signal Analysis with World Models`

## Apresentação

|Nome  | RA | Especialização|
|--|--|--|
| Davi Pincinato  | 157810  | Eng. Computação |
| Henrique Parede de Souza  | 260497  | Eng. Computação|
| Isadora Minuzzi Vieira  | 290184  | Eng. Biomédica|
| Raphael Carvalho da Silva e Silva  | 205125  | Eng. Computação |



## Descrição do projeto
O projeto propõe uma nova abordagem para a análise de sinais de áudio, adaptando o conceito de "modelos de mundo" do aprendizado por reforço para um domínio de sinais. O objetivo é desenvolver um modelo de aprendizado profundo que possa aprender representações e dinâmicas de dados de áudio a partir de seus espectrogramas.

A inspiração central vem das arquiteturas DreamerV2 e Dreaming, que resulta no modelo Dreaming V2. Enquanto o DreamerV2 se destaca ao aprender comportamentos em ambientes de jogos complexos, como o Atari, usando representações discretas de estados latentes, o Dreaming V2 estende essa ideia combinando a representação discreta com um objetivo de aprendizado livre de reconstrução. A principal adaptação é substituir as imagens de jogos por espectrogramas de áudio e tratar as janelas temporais do espectrograma como "tokens" discretos. Essa "tokenização" permite que o modelo capture a dinâmica temporal do áudio de uma forma que seja compatível com arquiteturas baseadas em atenção.

## Base de referência
Este projeto é baseado em duas referências principais:

#### 1. Artigo Acadêmico: ["Dreaming V2: Reinforcement Learning with Discrete World Models without Reconstruction"](https://arxiv.org/pdf/2203.00494).

**Principais contribuições/inspirações**: O artigo apresenta o Dreaming V2, uma extensão colaborativa do DreamerV2 e Dreaming. Ele adota a representação discreta do DreamerV2 e um objetivo livre de reconstrução do Dreaming. O modelo de mundo é treinado usando um aprendizado contrastivo, que elimina a necessidade de reconstruir observações visuais complexas. Os autores demonstraram que esta abordagem alcança resultados de última geração em tarefas de braços robóticos em 3D.

#### 2. Bases de Código para Implementação:
[dreamer-torch](https://github.com/jsikyoon/dreamer-torch): Implementação em PyTorch se assemelha ao código original do DreamerV2, que foi escrito em TensorFlow. É uma referência valiosa para entender a estrutura e a lógica do modelo em um framework amplamente utilizado na comunidade de pesquisa. Os resultados demonstraram desempenho similar ao do modelo original em tarefas de controle de jogos.

[pydreamer](https://github.com/jurgisp/pydreamer): Outra reimplementação do DreamerV2 em PyTorch, que introduz algumas diferenças sutis e melhorias, como o uso de processos separados para o trainer e os workers do ambiente, permitindo que o GPU seja utilizado totalmente. Esta base de código serve como um ponto de partida para explorar abordagens ligeiramente diferentes e otimizações.

### Metodologia da adaptação e implementação
A metodologia para adaptar o Dreaming V2 para dados de áudio envolverá as seguintes etapas:

#### 1. Pré-processamento de Áudio:

- Converter os arquivos de áudio brutos em espectrogramas (por exemplo, utilizando a Transformada de Fourier de Curto Tempo - STFT).

- Normalizar e pré-processar os espectrogramas para que sejam compatíveis com a entrada do modelo.

#### 2. "Tokenização" do Espectrograma:

- Ao invés de tratar o espectrograma como uma imagem 2D, ele será dividido em janelas temporais fixas. Cada janela se tornará um "token" que representa um segmento do áudio.

- Cada "token" será codificado em um vetor de alta dimensão (embedding) que será a entrada para o modelo de mundo.

#### 3. Arquitetura do Modelo de Mundo:

- A arquitetura será baseada no Recurrent State Space Model (RSSM), que define estados latentes estocásticos e determinísticos.

- O modelo de mundo será adaptado para processar a sequência de "tokens" de espectrograma. Ele consistirá em um codificador (encoder), um modelo de dinâmica latente (RSSM) e um decodificador (decoder). No entanto, o treinamento focará no objetivo de aprendizado contrastivo, eliminando a necessidade de reconstrução completa do espectrograma.

- O codificador transformará as janelas do espectrograma em um estado latente discreto, composto por variáveis categóricas.

- O RSSM será responsável por prever o próximo estado e recompensa (neste caso, a próxima janela de espectrograma) a partir do estado atual e de uma "ação" (pode ser um placeholder ou uma variável para representar a transição temporal).

#### 4. Treinamento do Modelo:

- O modelo será treinado usando um objetivo de aprendizado contrastivo, que compara pares positivos (previsão e observação correspondente) e negativos (previsão e observações diferentes) para aprender uma representação robusta sem a necessidade de uma tarefa de reconstrução explícita.

- O aprendizado será totalmente self-supervised, focado em aprender a dinâmica do mundo de áudio.

## Cronograma

Legenda: ▓ = duração da tarefa, ⭐ = entrega

| Fase de Trabalho       | Atividades Principais                           | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 |
|------------------------|-------------------------------------------------|---|---|---|---|---|---|---|---|---|----|----|
| Preparação & Setup     | Setup do ambiente + revisão código              | ▓ |   |   |   |   |   |   |   |   |    |    |
| Pré-processamento      | Conversão áudio → espectrograma + normalização  |   | ▓ |   |   |   |   |   |   |   |    |    |
| Pré-processamento      | Tokenização (janelas → embeddings)              |   | ▓ | ▓ |   |   |   |   |   |   |    |    |
| Modelo de Mundo        | Encoder + RSSM (ajuste usando DreamerV2)    |   |   | ▓ | ▓ |   |   |   |   |   |    |    |
| Modelo de Mundo        | Integração do Decoder / avaliação básica        |   |   |   |   | ▓ |   |   |   |   |    |    |
| **Entrega Parcial**    | Status do projeto                               |   |   |   |   |   | ⭐ |   |   |   |    |    |
| Treinamento            | Execução com DreamerV2 + ajustes leves      |   |   |   |   |   |   | ▓ | ▓ |   |    |    |
| Avaliação & Ajustes    | Análise de métricas e resultados                |   |   |   |   |   |   |   | ▓ | ▓ |    |    |
| Documentação           | Relatório + notebooks + apresentação            |   |   |   |   |   |   |   |   | ▓ | ▓  |    |
| **Entrega Final**      | Refinamento, validação final e entrega          |   |   |   |   |   |   |   |   |   |    | ⭐  |




## Tecnologias e bibliotecas utilizadas
**Linguagem**: Python

**Frameworks de Deep Learning**: PyTorch (conforme as bases de código de referência)

**Processamento de Áudio**: Librosa, Torchaudio

**Manipulação de Dados**: NumPy, Pandas

**Visualização**: Matplotlib, TensorBoard

## Links para a Apresentação

- Link para o [vídeo da apresentação](https://drive.google.com/file/d/1IFhNwxeS_8Gce3WTqXLOq8UJDLKJB7QQ/view?usp=sharing)
- Link para os [slides da apresentação](https://www.canva.com/design/DAGzF_vtvEE/6c1_5Sw-mUuLSqV6HMjP9Q/edit?utm_content=DAGzF_vtvEE&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton)

## Referências:
HA, David; SCHMIDHUBER, Jürgen. World Models. arXiv:1803.10122, 2018.
https://arxiv.org/abs/1803.10122

HAFNER, Danijar et al. DreamerV2: Mastering Atari with Discrete World Models. arXiv:2010.02193, 2020.
https://arxiv.org/abs/2010.02193

HAFNER, Danijar et al. Learning Latent Dynamics for Planning from Pixels (PlaNet). ICML, 2019.
https://arxiv.org/abs/1811.04551

OORD, Aaron van den et al. Neural Discrete Representation Learning (VQ-VAE). NeurIPS, 2017.
https://arxiv.org/abs/1711.00937

PRABHU, Kundan Kumar et al. Autoregressive Spectrogram Inpainting with Time–Frequency Transformers. arXiv preprint, 2021.
https://arxiv.org/abs/2104.03976

WANG, Yuxuan et al. Tacotron: Towards End-to-End Speech Synthesis. Interspeech, 2017.
https://arxiv.org/abs/1703.10135

PANAYOTOV, Vassil et al. LibriSpeech: An ASR Corpus. ICASSP, 2015.
https://www.openslr.org/12

Mozilla. Common Voice Dataset.
https://commonvoice.mozilla.org
