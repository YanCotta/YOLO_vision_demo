# Relatório de Projeto e Changelog: Detecção Customizada com YOLOv5

**Projeto:** Prova de Conceito de Visão Computacional para FarmTech Solutions
**Autor:** Yan Pimentel Cotta
**RM:** 562836
**Data de Conclusão:** 10 de Outubro de 2025
**Tecnologias:** Python, YOLOv5, PyTorch, Google Colab, Google Drive, Make Sense AI

---

### **1.0 Resumo Executivo**

Este documento descreve o desenvolvimento de uma Prova de Conceito (PoC) de visão computacional para a FarmTech Solutions, avaliando a arquitetura YOLOv5 na detecção dos objetos `banana` e `fork`. A jornada englobou coleta proprietária de dados, rotulagem, treinamento, avaliação, inferência e documentação de governança. Foram conduzidos três experimentos controlados — dois com a arquitetura **YOLOv5s** (30 e 60 épocas) e um com a arquitetura **YOLOv5m** (60 épocas). O modelo campeão (`YOLOv5m`) alcançou **mAP@.50 geral de 0.789**, com desempenho praticamente perfeito na classe `banana` (0.995) e avanço significativo na classe `fork` (0.584). O estudo confirmou que arquiteturas mais robustas mitigam a limitação das bounding boxes para objetos de geometria complexa, ao custo de um artefato final maior (42 MB). As conclusões orientam decisões distintas para cenários cloud versus edge e delineiam um roadmap de evolução tecnológica.

---

### **2.0 Metodologia e Plano de Ação**

Para garantir uma execução profissional, organizada e reprodutível, o projeto foi segmentado em fases distintas, seguindo um pipeline padrão de desenvolvimento de modelos de Machine Learning:

* **Fase 0: Setup e Fundação do Ambiente:** Configuração do versionamento de código (Git), da estrutura de armazenamento de dados (Google Drive) e do ambiente de desenvolvimento (Colab).
* **Fase 1: Aquisição e Preparação dos Dados:** Seleção estratégica dos objetos e coleta de um dataset de imagens com variabilidade controlada.
* **Fase 2: Anotação dos Dados (Labeling):** Processo de rotulação manual das imagens para gerar as "respostas" para o treinamento supervisionado.
* **Fase 3: Desenvolvimento e Treinamento:** Implementação do pipeline YOLOv5 com três experimentos (YOLOv5s 30 épocas, YOLOv5s 60 épocas, YOLOv5m 60 épocas) para medir o impacto de tempo de treino e complexidade arquitetural.
* **Fase 4: Análise e Avaliação:** Análise quantitativa e qualitativa dos resultados dos modelos treinados para selecionar o de melhor performance.
* **Fase 5: Inferência e Validação Final:** Teste do modelo campeão em um conjunto de dados nunca visto para validar sua capacidade de generalização.
* **Fase 6: Empacotamento e Entrega:** Finalização da documentação, geração dos artefatos (vídeo, README) e submissão do projeto.

---

### **3.0 Execução Detalhada do Projeto (Changelog)**

* **Setup do Ambiente:**
    * Inicializado repositório no GitHub com template `.gitignore` para Python, garantindo que apenas o código-fonte e a documentação fossem versionados.
    * Estruturada uma hierarquia de pastas no Google Drive (`/datasets/images/` e `/datasets/labels/`) para garantir a separação limpa entre dados brutos e anotações, e para facilitar a integração com o Colab e o YOLOv5.

* **Aquisição de Dados:**
    * Selecionadas as classes `banana` e `fork` devido à sua alta distinção visual e disponibilidade física, pensando na futura implementação do projeto "Ir Além 1" com ESP32-CAM.
    * Coletado um dataset com 80 imagens (40 por classe), priorizando a variabilidade de ângulos, iluminação e fundos para aumentar a robustez do modelo.
    * O dataset foi dividido na proporção 80/10/10 (Treino/Validação/Teste), resultando em 64 imagens para treino, 8 para validação e 8 para teste.

* **Anotação de Dados:**
    * Utilizada a plataforma Make Sense AI para a anotação. As 72 imagens dos conjuntos de treino e validação foram rotuladas.

* **Treinamento e Experimentação:**
    * Desenvolvido um notebook em Google Colab com narrativa de relatório e monitoramento visual das métricas.
    * Implementado o pipeline de treinamento do YOLOv5, utilizando transfer learning a partir dos pesos `yolov5s.pt` e `yolov5m.pt`.
    * Conduzidos três experimentos controlados: YOLOv5s (30 épocas), YOLOv5s (60 épocas) e YOLOv5m (60 épocas), comparando acurácia, custo computacional e tamanho do modelo.

---

### **4.0 Desafios Encontrados e Soluções Aplicadas**

* **Desafio 1: Incompatibilidade de Anotação (Polígonos vs. Bounding Boxes)**
    * **Problema:** A primeira tentativa de anotação foi realizada com polígonos para maximizar a precisão. Contudo, a ferramenta Make Sense AI não permitia a exportação de polígonos para o formato YOLO, que é estritamente baseado em `bounding box`.
    * **Solução:** Foi necessário re-anotar todo o dataset (72 imagens) utilizando a ferramenta de `bounding box`.
    * **Aprendizado:** Este incidente reforçou a necessidade crítica de alinhar a metodologia de anotação com os requisitos de entrada do modelo *antes* do início do trabalho manual, uma lição valiosa para a otimização de pipelines de dados.

* **Desafio 2: Perda de Contexto de Diretório no Colab**
    * **Problema:** Após interromper uma célula de treinamento, o ambiente Colab perdeu o contexto do diretório de trabalho (`/content/yolov5/`), resultando em um erro de `FileNotFoundError` ao tentar executar novamente o script `train.py`.
    * **Solução:** O notebook foi tornado mais robusto ao adicionar o comando `%cd /content/yolov5/` no início de todas as células de execução de scripts. Isso garante que o comando seja sempre executado a partir do diretório correto, independentemente do estado anterior da sessão.

* **Desafio 3: Formato de Imagem Inválido (`.AVIF`)**
    * **Problema:** Durante a execução do treinamento, o log do YOLOv5 emitiu avisos de que duas imagens (`.AVIF`) não puderam ser lidas e foram ignoradas.
    * **Solução:** As imagens foram identificadas e um plano de ação foi criado para convertê-las para um formato padrão (`.jpg` ou `.png`) e atualizar o dataset. O treinamento prosseguiu com o dataset reduzido, mas a correção garante a integridade dos dados para futuras iterações e a reprodutibilidade do experimento.

---

### **5.0 Análise Sênior de Resultados**

A evolução de desempenho evidenciou dois pontos principais:

1. **Impacto do Tempo de Treinamento:** o salto de 30 para 60 épocas no `YOLOv5s` elevou o mAP@.50 geral de 0.393 para 0.513 (+30,5%), eliminando sinais de underfitting observados no experimento inicial.
2. **Impacto da Complexidade do Modelo:** a migração para o `YOLOv5m` elevou o mAP@.50 geral para 0.789 (+53,8% sobre o melhor modelo pequeno) e mais que dobrou o desempenho em `fork` (0.262 → 0.584). O custo foi um aumento de ~200% no tamanho do modelo (14 MB → 42 MB), implicando considerações específicas de deploy.

A análise qualitativa em imagens inéditas reforçou a hipótese de que objetos de geometria irregular sofrem com a anotação em bounding boxes. Mesmo com a arquitetura média, ainda há ruído residual, apontando para a necessidade de segmentação de instâncias ou técnicas complementares de enriquecimento de dados para maximizar o resultado.

---

### **6.0 Recomendações Estratégicas**

1. **Ambientes com recursos amplos (cloud/desktop):** implantar o modelo `YOLOv5m`, que entrega melhor equilíbrio entre acurácia e tempo de inferência.
2. **Dispositivos embarcados (edge restrito):** manter o `YOLOv5s` como baseline e avaliar compressão (quantização, pruning) para aproximar o desempenho do modelo médio sem inviabilizar o hardware.
3. **Evolução técnica:** ampliar o dataset, intensificar data augmentation, executar busca de hiperparâmetros e iniciar provas de conceito com segmentação de instâncias (Mask R-CNN, YOLACT) para objetos irregulares.
4. **Governança e MLOps:** estabelecer controle de versões dos experimentos, monitoramento contínuo de métricas e checklist de qualidade de dados para garantir reproducibilidade a cada ciclo.
