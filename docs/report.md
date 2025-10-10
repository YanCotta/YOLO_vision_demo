# Relatório de Projeto e Changelog: Detecção Customizada com YOLOv5

**Projeto:** Prova de Conceito de Visão Computacional para FarmTech Solutions
**Autor:** Yan Pimentel Cotta
**RM:** 562836
**Data de Conclusão:** 10 de Outubro de 2025
**Tecnologias:** Python, YOLOv5, PyTorch, Google Colab, Google Drive, Make Sense AI

---

### **1.0 Resumo Executivo**

Este documento detalha o ciclo de vida completo do desenvolvimento de um protótipo de detecção de objetos customizado, desde a concepção até a análise final. O objetivo foi validar a viabilidade da arquitetura YOLOv5 para identificar objetos específicos (`banana`, `fork`) em um ambiente controlado. O projeto foi executado com sucesso, resultando em um modelo funcional e, mais crucialmente, gerando insights estratégicos sobre as limitações da metodologia de anotação por `bounding box` para objetos de geometria complexa. O modelo final demonstrou alta performance na detecção de objetos regulares (100% de sucesso em `banana`) e performance insuficiente em objetos irregulares (25% de sucesso em `fork`), validando a hipótese de que uma mudança de paradigma tecnológico (para Segmentação de Instâncias) é necessária para escalar a solução.

---

### **2.0 Metodologia e Plano de Ação**

Para garantir uma execução profissional, organizada e reprodutível, o projeto foi segmentado em fases distintas, seguindo um pipeline padrão de desenvolvimento de modelos de Machine Learning:

* **Fase 0: Setup e Fundação do Ambiente:** Configuração do versionamento de código (Git), da estrutura de armazenamento de dados (Google Drive) e do ambiente de desenvolvimento (Colab).
* **Fase 1: Aquisição e Preparação dos Dados:** Seleção estratégica dos objetos e coleta de um dataset de imagens com variabilidade controlada.
* **Fase 2: Anotação dos Dados (Labeling):** Processo de rotulação manual das imagens para gerar as "respostas" para o treinamento supervisionado.
* **Fase 3: Desenvolvimento e Treinamento:** Implementação do código para treinar o modelo YOLOv5, incluindo a condução de múltiplos experimentos para otimização.
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
    * Desenvolvido um notebook em Google Colab com uma estrutura de relatório profissional usando células Markdown.
    * Implementado o pipeline de treinamento do YOLOv5, utilizando transfer learning a partir dos pesos `yolov5s.pt` para acelerar a convergência.
    * Conduzidos dois experimentos controlados: um com 30 épocas e outro com 60 épocas, para avaliar o impacto da duração do treinamento.

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

A análise comparativa entre os dois modelos treinados revelou que o modelo de **60 épocas** foi superior em todas as métricas, com um aumento de **30.5% no mAP@.50 geral**. A análise de inferência no conjunto de teste expôs uma performance dicotômica: o modelo obteve **100% de sucesso na detecção de `banana`**, mas apenas **25% de sucesso na detecção de `fork`**.

Esta disparidade valida a hipótese de que a anotação por `bounding box` é fundamentalmente inadequada para objetos de geometria complexa. A caixa delimitadora de um garfo possui uma baixa razão sinal-ruído, capturando mais pixels de fundo do que do objeto em si, o que confunde o modelo e impede o aprendizado de características robustas. Este insight, embora demonstre uma limitação do modelo, representa o ganho intelectual mais valioso do projeto.

---

### **6.0 Recomendações Estratégicas**

Com base nos resultados, a recomendação estratégica para a evolução deste sistema é dupla:
1.  **Iteração de Curto Prazo:** Para melhorar o modelo YOLOv5 existente, é recomendado um enriquecimento massivo do dataset e a realização de um ciclo de ajuste de hiperparâmetros.
2.  **Evolução Estratégica:** Para lidar eficazmente com objetos de geometria complexa, é fortemente recomendada a transição do paradigma de Detecção de Objetos para **Segmentação de Instâncias**. A implementação de arquiteturas como **Mask R-CNN** resolveria a questão da razão sinal-ruído e permitiria uma detecção muito mais precisa e robusta de uma gama maior de objetos.