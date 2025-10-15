# FIAP - Faculdade de Informática e Administração Paulista

<p align="center">
<a href= "https://www.fiap.com.br/"><img src="assets/logo-fiap.png" alt="FIAP - Faculdade de Informática e Admnistração Paulista" border="0" width=40% height=40%></a>
</p>

<br>

## Projeto: Detecção Customizada com YOLOv5

### Nome do grupo

FarmTech Vision Lab

## 👨‍🎓 Integrantes

- [Yan Pimentel Cotta - RM: 562836](https://www.linkedin.com/in/yan-cotta)
- [Jonas T V Fernandes - RM: 563027](https://www.linkedin.com/in/jonastadeufernandes)
- [Raphael da Silva - RM: 561452](https://www.linkedin.com/in/raphaelsilva-phael)
- [Raphael Dinelli Neto - RM: 562892](https://www.linkedin.com/in/raphael-dinelli-8a01b278/)
- [Levi Passos Silveira Marques - RM: 565557](https://www.linkedin.com/company/inova-fusca)

## 👩‍🏫 Professores

### Tutor(a)

- [Leonardo Ruiz Orabona](https://www.linkedin.com/in/leonardoorabona)

### Coordenador(a)

- [André Godoi](https://www.linkedin.com/in/andregodoichiovato/)
 
## 📜 Descrição

Este projeto apresenta uma solução completa de visão computacional desenvolvida para a FarmTech Solutions, englobando três entregas distintas que demonstram diferentes aspectos e técnicas de detecção e classificação de objetos utilizando Deep Learning.

### Entrega 1: Detecção Customizada com YOLOv5
- Prova de conceito de visão computacional utilizando YOLOv5 para detectar objetos customizados (`banana` e `fork`).
- Pipeline completo: coleta proprietária de 80 imagens (40 por classe), rotulagem no Make Sense AI, treinamento em Google Colab, avaliação quantitativa, validação qualitativa e documentação executiva.
- Três experimentos controlados:
  - **YOLOv5s com 30 épocas**: mAP@.50 de 0.393 (baseline inicial)
  - **YOLOv5s com 60 épocas**: mAP@.50 de 0.513 (melhora de 30,5%)
  - **YOLOv5m com 60 épocas**: mAP@.50 de 0.789 (modelo campeão, +53,8%)
- Análise aprofundada dos trade-offs entre acurácia e custo computacional, com foco em implicações para deploy em ambientes cloud versus edge.
- Discussão detalhada sobre limitações de bounding boxes para objetos de geometria complexa e roadmap de evolução (segmentação, otimização de hiperparâmetros, compressão de modelos).
- **Notebook**: [`notebooks/entregavel_1_fase6_cap1.ipynb`](notebooks/entregavel_1_fase6_cap1.ipynb)
- **Vídeo explicativo**: [https://www.youtube.com/watch?v=Z7gbPLNvm-4]

### Entrega 2: Comparação YOLO vs CNN
- Estudo comparativo entre a arquitetura YOLOv5 (detecção de objetos) e uma Rede Neural Convolucional customizada (classificação de imagens).
- Desenvolvimento de uma CNN do zero com arquitetura sequencial, incluindo camadas convolucionais, pooling, dropout e densas.
- Treinamento da CNN em dois cenários: 30 épocas e 60 épocas, avaliando acurácia de treino e validação.
- Análise comparativa considerando:
  - Facilidade de uso e integração
  - Precisão e métricas de desempenho
  - Tempo de treinamento e customização
  - Tempo de inferência
- Conclusão: YOLOv5 demonstrou superioridade para tarefas de detecção em tempo real, enquanto a CNN é mais adequada para classificação pura.
- **Notebook**: [`notebooks/Entrega2_RaphaelDaSilva_RM561452_fase6_cap1.ipynb`](notebooks/Entrega2_RaphaelDaSilva_RM561452_fase6_cap1.ipynb)
- **Vídeo explicativo**: [https://youtu.be/z1lbYlWnz9I]

### Ir Além 2: Classificação Avançada com Transfer Learning
- Pipeline avançado de duas etapas combinando detecção de objetos e classificação com transfer learning.
- **Abordagem 1 (Baseline)**: Classificador direto usando MobileNetV2 pré-treinado para classificação de imagens.
- **Abordagem 2 (Pipeline Integrado)**: Pré-processamento inteligente com YOLOv5 para identificar e recortar ROI (Região de Interesse), seguido de classificação com MobileNetV2.
- Resultados experimentais:
  - Baseline: 75% acurácia, 67.86% precisão, 100% recall
  - Pipeline Integrado: 87.50% acurácia (+12.50 pontos), 85.71% precisão (+17.86 pontos), 75% recall
- Validação da hipótese de que o pré-processamento com detecção melhora significativamente a acurácia e precisão da classificação.
- Análise de trade-offs entre confiabilidade (precisão) e abrangência (recall), com recomendações para diferentes contextos de aplicação.
- **Notebook**: [`notebooks/ir_alem_opcao_2_fase_6_cap1.ipynb`](notebooks/ir_alem_opcao_2_fase_6_cap1.ipynb)
- **Documentação adicional**: [`README_IR_ALEM_2.md`](README_IR_ALEM_2.md)
- **Vídeo explicativo**: [https://www.youtube.com/watch?v=csRnLQyvWsM]

### Documentação Integrada
- Relatório executivo detalhado: [`docs/report.md`](docs/report.md)
- Orientações do projeto: [`docs/orientation.md`](docs/orientation.md)
- Gráficos comparativos, tabelas executivas e análises textuais disponíveis nos notebooks

## 💽 Fontes de dados

- **Dataset proprietário**: 80 imagens capturadas manualmente (40 imagens de `banana` e 40 imagens de `fork`).
- **Divisão estratificada**: 64 imagens para treino, 8 para validação, 8 para teste (proporção 80/10/10).
- **Rotulagem**: Anotação manual realizada no Make Sense AI com bounding boxes retangulares para compatibilidade com o formato YOLO.
- **Armazenamento**: Imagens e rótulos armazenados no Google Drive para integração com Google Colab.
- **Variabilidade**: Dataset coletado com diversidade de ângulos, iluminação e fundos para aumentar a robustez dos modelos.

## 📁 Estrutura de pastas

```
YOLO_vision_demo/
├── notebooks/
│   ├── entregavel_1_fase6_cap1.ipynb              # Entrega 1: Detecção YOLOv5
│   ├── Entrega2_RaphaelDaSilva_RM561452_fase6_cap1.ipynb  # Entrega 2: YOLO vs CNN
│   └── ir_alem_opcao_2_fase_6_cap1.ipynb          # Ir Além 2: Transfer Learning
├── docs/
│   ├── orientation.md          # Enunciado oficial e requisitos do projeto
│   └── report.md              # Relatório executivo detalhado (Entrega 1)
├── assets/                    # Logotipos e materiais visuais de apoio
├── README.md                  # Este arquivo - visão geral do projeto
└── README_IR_ALEM_2.md       # Documentação específica do "Ir Além 2"
```

## 🔧 Como executar o código

### Entrega 1: Detecção YOLOv5
1. Abra o notebook [`notebooks/entregavel_1_fase6_cap1.ipynb`](notebooks/entregavel_1_fase6_cap1.ipynb) no Google Colab.
2. Monte o Google Drive e ajuste os caminhos conforme necessário (`/content/drive/MyDrive/PBL6_Project_YOLO/`).
3. Execute as células sequencialmente:
   - Clonagem do repositório YOLOv5
   - Instalação de dependências
   - Montagem e configuração do dataset
   - Treinamentos dos três experimentos (Exp_30_Epocas, Exp_60_Epocas, Exp_60_Epocas_Medium)
   - Inferências no conjunto de teste
4. Monitore os resultados em:
   - `runs/train/Exp_30_Epocas` - Primeiro experimento (YOLOv5s, 30 épocas)
   - `runs/train/Exp_60_Epocas` - Segundo experimento (YOLOv5s, 60 épocas)
   - `runs/train/Exp_60_Epocas_Medium` - Terceiro experimento (YOLOv5m, 60 épocas)
   - `runs/detect/Teste_Final` - Resultados da inferência
5. Exporte os gráficos comparativos gerados:
   - `comparacao_curvas_aprendizado.png`
   - `comparativo_final_barras.png`
   - `comparacao_curvas_de_perda.png`
   - `tabela_comparativa_final.png`

### Entrega 2: Comparação YOLO vs CNN
1. Abra o notebook [`notebooks/Entrega2_RaphaelDaSilva_RM561452_fase6_cap1.ipynb`](notebooks/Entrega2_RaphaelDaSilva_RM561452_fase6_cap1.ipynb) no Google Colab.
2. Monte o Google Drive com o dataset já preparado.
3. Execute as seções do notebook:
   - Configuração e importações
   - Treinamento do modelo YOLOv5 tradicional
   - Construção e treinamento da CNN customizada (30 e 60 épocas)
   - Análise visual dos resultados de ambos os modelos
   - Comparação de métricas e conclusões
4. Revise os gráficos de perda, acurácia e exemplos de inferência gerados.

### Ir Além 2: Transfer Learning Avançado
1. Abra o notebook [`notebooks/ir_alem_opcao_2_fase_6_cap1.ipynb`](notebooks/ir_alem_opcao_2_fase_6_cap1.ipynb) no Google Colab.
2. Monte o Google Drive e configure os caminhos do dataset.
3. Execute as seções do notebook:
   - Setup do ambiente e preparação dos dados
   - **Abordagem 1**: Treinamento do classificador baseline com MobileNetV2
   - **Abordagem 2**: Implementação do pipeline integrado (YOLOv5 + MobileNetV2)
   - Análise comparativa entre as duas abordagens
   - Visualizações e conclusões
4. Analise os resultados comparativos de acurácia, precisão e recall entre as abordagens.

### Observações Gerais
- Todos os notebooks foram desenvolvidos para execução no **Google Colab** com GPU T4.
- É necessário ter uma conta Google com acesso ao Google Drive para armazenamento do dataset.
- Os notebooks contêm células Markdown detalhadas explicando cada etapa do processo.
- Recomenda-se executar as células sequencialmente para evitar erros de dependências.

## 🗃 Histórico de lançamentos

### Entregas Principais
- **2025-10-10 - Entrega 1**: Prova de conceito inicial com YOLOv5, incluindo três experimentos comparativos (YOLOv5s 30 épocas, YOLOv5s 60 épocas, YOLOv5m 60 épocas), documentação executiva completa e análise de trade-offs para deploy em diferentes ambientes.

- **2025-10-13 - Entrega 2**: Estudo comparativo entre YOLOv5 e CNN customizada, avaliando diferentes arquiteturas para detecção versus classificação de objetos, incluindo análise de desempenho, facilidade de uso e aplicabilidade.

- **2025-10-15 - Ir Além 2**: Pipeline avançado integrando YOLOv5 para detecção e MobileNetV2 para classificação, demonstrando técnicas de transfer learning e pré-processamento inteligente com análise comparativa quantitativa.

### Atualizações de Documentação
- **2025-10-15**: Atualização completa do README.md e docs/report.md refletindo o escopo integral do projeto com todas as três entregas em português brasileiro.

## 📋 Licença

Este repositório segue o modelo [MODELO GIT FIAP](https://github.com/agodoi/template) licenciado sob [Creative Commons Attribution 4.0 International](http://creativecommons.org/licenses/by/4.0/?ref=chooser-v1).
