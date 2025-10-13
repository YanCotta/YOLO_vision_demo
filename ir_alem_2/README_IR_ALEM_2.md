# Plano de Execução Detalhado: Projeto "Ir Além 2"

**Projeto:** Classificação Avançada de Imagens com Transfer Learning e Pré-processamento Inteligente
**Autor:** Yan Pimentel Cotta
**RM:** 562836
**Objetivo:** Este documento serve como um roteiro técnico para a execução do entregável "Ir Além 2". O objetivo é implementar e avaliar criticamente duas abordagens avançadas para a classificação de imagens (`banana` vs. `fork`), comparando um pipeline de Transfer Learning padrão com um pipeline aprimorado que utiliza nosso modelo de detecção de objetos YOLOv5 como uma etapa de pré-processamento inteligente.

**Contexto para o Agente Copilot:**

  * Este projeto é a continuação do `Entregável 1`, cujo notebook (`YanPimentelCotta_RM562836_fase6_cap1.ipynb`) está localizado na pasta `/notebooks` deste repositório.
  * O dataset original, com as imagens de bananas e garfos, reside no Google Drive do usuário. A estrutura exata e os caminhos precisarão ser confirmados pelo usuário.
  * O objetivo é gerar um único notebook Jupyter (`YanPimentelCotta_RM562836_Ir_Alem_2.ipynb`) que contenha todo o código, análise e visualizações descritas neste plano.

-----

## Fase 1: Setup e Abordagem 1 - Transfer Learning com MobileNetV2 (Baseline)

**Objetivo da Fase:** Estabelecer uma base de performance (baseline) sólida para nossa tarefa de classificação, utilizando uma arquitetura de Transfer Learning padrão e eficiente.

### **Tarefa 1.1: Estruturação do Notebook**

  * **Ação:** Crie um novo notebook Jupyter com o nome `YanPimentelCotta_RM562836_Ir_Alem_2.ipynb`.
  * **Ação (Copilot):** Popule o notebook com a seguinte estrutura de títulos em células Markdown para criar o esqueleto do relatório:
    ```markdown
    # Projeto "Ir Além 2": Classificação Avançada com Transfer Learning e Pré-processamento YOLOv5
    **Autor:** Yan Pimentel Cotta
    **RM:** 562836
    ---
    ## 1.0 Setup do Ambiente e Preparação dos Dados
    *Nesta seção, configuramos o ambiente, conectamos ao Google Drive e preparamos os datasets para a tarefa de classificação.*
    ---
    ## 2.0 Abordagem 1: Classificação com Transfer Learning (Baseline)
    *Implementamos e treinamos um classificador usando a arquitetura MobileNetV2 para estabelecer nossa linha de base de performance.*
    ---
    ## 3.0 Abordagem 2: Pipeline de Pré-processamento com YOLOv5
    *Desenvolvemos uma função para pré-processar as imagens, utilizando nosso modelo YOLOv5 treinado para identificar e recortar a Região de Interesse (ROI).*
    ---
    ## 4.0 Análise Comparativa e Validação de Hipóteses
    *Avaliamos o classificador da Abordagem 1 nas imagens pré-processadas e comparamos os resultados para validar nossas hipóteses.*
    ---
    ## 5.0 Conclusão e Diagrama de Arquitetura
    *Consolidamos os aprendizados, justificamos as decisões de engenharia e apresentamos a arquitetura final do nosso pipeline mais avançado.*
    ```

### **Tarefa 1.2: Preparação Automatizada do Dataset de Classificação**

  * **Contexto:** O TensorFlow/Keras carrega dados de classificação de forma eficiente a partir de uma estrutura de pastas específica (`/classe/imagem.jpg`). Precisamos transformar nosso dataset original nesta estrutura.

  * **Ação (Copilot):** Na seção `1.0`, adicione uma célula de código para montar o Google Drive e automatizar a criação da estrutura de pastas e a cópia das imagens.

    ```python
    # Importações iniciais
    import os
    import shutil
    from google.colab import drive

    # Monta o Google Drive
    drive.mount('/content/drive')

    # --- AGENTE, CONSULTE O USUÁRIO ABAIXO ---
    # Por favor, peça ao usuário para confirmar ou corrigir o caminho base para a pasta do projeto no Google Drive.
    base_drive_path = '/content/drive/MyDrive/PBL6_Project_YOLO/datasets'
    # -----------------------------------------

    source_images_path = os.path.join(base_drive_path, 'images')
    dest_classification_path = os.path.join(base_drive_path, 'classification')

    print(f"Caminho de origem das imagens: {source_images_path}")
    print(f"Caminho de destino para classificação: {dest_classification_path}")

    # Cria a estrutura de pastas de destino
    if not os.path.exists(dest_classification_path):
        os.makedirs(dest_classification_path)
        print(f"Diretório criado: {dest_classification_path}")
        for split in ['train', 'valid', 'test']:
            for class_name in ['banana', 'fork']:
                os.makedirs(os.path.join(dest_classification_path, split, class_name), exist_ok=True)
        print("Estrutura de pastas para classificação criada com sucesso.")
    else:
        print("Estrutura de pastas para classificação já existe.")

    # Copia as imagens para a nova estrutura
    # Este bloco assume que os nomes dos arquivos indicam a classe (ex: 'banana01.png', 'garfo32.png')
    # AGENTE: Este código precisa ser robusto.
    print("\nIniciando a cópia dos arquivos...")
    for split in ['train', 'valid', 'test']:
        split_source_path = os.path.join(source_images_path, split)
        for filename in os.listdir(split_source_path):
            if 'banana' in filename.lower():
                class_name = 'banana'
            elif 'fork' in filename.lower() or 'garfo' in filename.lower():
                class_name = 'fork'
            else:
                continue # Pula arquivos que não correspondem

            source_file = os.path.join(split_source_path, filename)
            destination_folder = os.path.join(dest_classification_path, split, class_name)
            shutil.copy(source_file, destination_folder)

    print("Cópia de arquivos para a estrutura de classificação concluída.")
    ```

### **Tarefa 1.3: Implementação do Pipeline de Transfer Learning (Baseline)**

  * **Contexto:** Utilizaremos a `MobileNetV2` como nossa arquitetura base. É uma escolha estratégica que equilibra alta performance e eficiência computacional.

  * **Ação (Copilot):** Na seção `2.0`, adicione as células de código para construir, treinar e avaliar o modelo de Transfer Learning.

    ```python
    # Importações para o modelo
    import tensorflow as tf
    import matplotlib.pyplot as plt

    # Carrega os dados usando a API do Keras
    IMG_SIZE = (224, 224)
    BATCH_SIZE = 32 # Um batch size maior é possível aqui, pois não estamos treinando do zero

    train_dataset = tf.keras.utils.image_dataset_from_directory(
        os.path.join(dest_classification_path, 'train'),
        shuffle=True,
        batch_size=BATCH_SIZE,
        image_size=IMG_SIZE
    )
    #... (código similar para validation_dataset e test_dataset) ...

    # Otimização do pipeline de dados
    AUTOTUNE = tf.data.AUTOTUNE
    train_dataset = train_dataset.prefetch(buffer_size=AUTOTUNE)
    #... (código similar para validation_dataset e test_dataset) ...

    # Criação do modelo
    # 1. Pré-processamento de entrada específico da MobileNetV2
    preprocess_input = tf.keras.applications.mobilenet_v2.preprocess_input
    # 2. Carregamento do modelo base sem o topo
    base_model = tf.keras.applications.MobileNetV2(input_shape=(224, 224, 3),
                                                   include_top=False,
                                                   weights='imagenet')
    # 3. Congelamento do modelo base
    base_model.trainable = False

    # 4. Construção do nosso modelo completo
    inputs = tf.keras.Input(shape=(224, 224, 3))
    x = preprocess_input(inputs)
    x = base_model(x, training=False)
    x = tf.keras.layers.GlobalAveragePooling2D()(x)
    x = tf.keras.layers.Dropout(0.2)(x) # Camada de regularização para evitar overfitting
    outputs = tf.keras.layers.Dense(1, activation='sigmoid')(x) # Saída binária
    model = tf.keras.Model(inputs, outputs)

    # 5. Compilação do modelo
    model.compile(optimizer=tf.keras.optimizers.Adam(learning_rate=0.0001),
                  loss=tf.keras.losses.BinaryCrossentropy(),
                  metrics=['accuracy'])

    model.summary()

    # Treinamento
    history = model.fit(train_dataset,
                        epochs=25,
                        validation_data=validation_dataset)

    # Avaliação final no conjunto de teste
    loss_baseline, accuracy_baseline = model.evaluate(test_dataset)
    print(f"Acurácia da Baseline no conjunto de teste: {accuracy_baseline:.2%}")
    ```

-----

## Fase 2: Abordagem 2 - Classificação com Pré-processamento YOLOv5

**Objetivo da Fase:** Desenvolver um pipeline que utilize nosso melhor modelo YOLOv5 para isolar a Região de Interesse (ROI) das imagens antes de passá-las ao classificador, testando a hipótese de que isso melhora a acurácia.

### **Tarefa 2.1: Carregamento do Modelo YOLOv5**

  * **Contexto:** Precisamos carregar o nosso melhor modelo treinado (`best.pt` do `Exp_60_Epocas_Medium`) do Google Drive para o ambiente Colab. Este modelo foi treinado com PyTorch.

  * **Ação (Copilot):** Na seção `3.0`, adicione uma célula para instalar o PyTorch e carregar o modelo.

    ```python
    import torch
    import cv2
    import numpy as np

    # --- AGENTE, CONSULTE O USUÁRIO ABAIXO ---
    # Por favor, peça ao usuário para confirmar ou corrigir o caminho para o arquivo 'best.pt' do melhor modelo YOLOv5 no Google Drive.
    yolo_model_path = '/content/drive/MyDrive/PBL6_Project_YOLO/yolov5/runs/train/Exp_60_Epocas_Medium/weights/best.pt'
    # -----------------------------------------

    # Carrega o modelo YOLOv5 a partir do hub do PyTorch
    # É preciso especificar o caminho para o repositório local do yolov5 para que ele encontre a arquitetura
    yolo_model = torch.hub.load('/content/yolov5', 'custom', path=yolo_model_path, source='local')
    print("Modelo YOLOv5 carregado com sucesso.")
    ```

### **Tarefa 2.2: Implementação da Função de Recorte Inteligente**

  * **Contexto:** Esta função será o coração da nossa segunda abordagem. Ela irá orquestrar a detecção e o recorte.

  * **Ação (Copilot):** Adicione o código para a função de recorte.

    ```python
    def crop_object_from_image(image_path, yolo_model):
        """
        Detecta um objeto em uma imagem usando um modelo YOLOv5 e retorna a imagem recortada.
        """
        img = cv2.imread(image_path)
        if img is None:
            return None

        # Realiza a inferência
        results = yolo_model(img)
        # Extrai as coordenadas da detecção com maior confiança
        detections = results.xyxy[0] # Formato (xmin, ymin, xmax, ymax, confidence, class)
        if len(detections) > 0:
            best_detection = detections[detections[:, 4].argmax()] # Pega a detecção com maior confiança
            xmin, ymin, xmax, ymax = map(int, best_detection[:4])

            # Recorta a imagem usando as coordenadas
            cropped_img = img[ymin:ymax, xmin:xmax]
            return cropped_img
        else:
            # Se nada for detectado, retorna a imagem original para não perder o dado
            return img
    ```

### **Tarefa 2.3: Demonstração Visual e Aplicação ao Dataset de Teste**

  * **Ação (Copilot):** Adicione código para demonstrar o pipeline e aplicá-lo ao conjunto de teste.

    ```python
    # Demonstração visual
    # --- AGENTE, CONSULTE O USUÁRIO ABAIXO ---
    # Peça ao usuário um caminho de exemplo de uma imagem de teste.
    example_image_path = '/content/drive/MyDrive/PBL6_Project_YOLO/datasets/images/test/banana37.png'
    # -----------------------------------------
    cropped_example = crop_object_from_image(example_image_path, yolo_model)

    plt.figure(figsize=(12, 6))
    plt.subplot(1, 2, 1)
    plt.title("Imagem Original")
    plt.imshow(cv2.cvtColor(cv2.imread(example_image_path), cv2.COLOR_BGR2RGB))
    plt.subplot(1, 2, 2)
    plt.title("Imagem Recortada pelo YOLOv5")
    plt.imshow(cv2.cvtColor(cropped_example, cv2.COLOR_BGR2RGB))
    plt.show()

    # Aplica o recorte a todo o conjunto de teste
    # ... (código para iterar sobre as imagens de teste, aplicar a função e salvar em uma nova pasta) ...
    ```

-----

## Fase 3: Análise Comparativa e Finalização

**Objetivo da Fase:** Avaliar a eficácia do nosso pipeline aprimorado e consolidar todos os aprendizados em uma conclusão final e profissional.

### **Tarefa 3.1: Avaliação da Abordagem 2**

  * **Ação (Copilot):** Na seção `4.0`, adicione código para carregar o dataset de teste recortado e avaliá-lo com o classificador `MobileNetV2` **já treinado**.

    ```python
    # Carrega o dataset de teste recortado
    test_cropped_dataset = tf.keras.utils.image_dataset_from_directory(
        # ... (caminho para a pasta de teste com imagens recortadas) ...
    )
    test_cropped_dataset = test_cropped_dataset.prefetch(buffer_size=AUTOTUNE)

    # Avalia o modelo treinado na Fase 1 com os novos dados
    loss_yolo_crop, accuracy_yolo_crop = model.evaluate(test_cropped_dataset)
    print(f"Acurácia com Pré-processamento YOLOv5 no conjunto de teste: {accuracy_yolo_crop:.2%}")
    ```

### **Tarefa 3.2: Análise Final e Conclusão**

  * **Ação (Copilot):** Na seção `5.0`, adicione uma célula Markdown com a seguinte estrutura. Os valores de acurácia devem ser preenchidos com os resultados obtidos.

    ```markdown
    ## 5.0 Análise Comparativa e Conclusão

    ### Tabela Comparativa de Resultados
    | Abordagem                                   | Acurácia no Conjunto de Teste | Análise de Complexidade                               |
    | ------------------------------------------- | ----------------------------- | ----------------------------------------------------- |
    | **1: Transfer Learning (Baseline)** | `[resultado da accuracy_baseline]` | Pipeline simples, dependente da qualidade da imagem inteira. |
    | **2: YOLOv5 Crop + Transfer Learning** | `[resultado da accuracy_yolo_crop]` | Pipeline mais complexo, duas etapas (detecção + classificação), mas focado na ROI. |

    ### Validação da Hipótese e Conclusão Final
    **Hipótese a ser validada:** *"Pré-segmentar o objeto de interesse... facilita a classificação da imagem?"*

    **Análise:**
    * **AGENTE:** Insira aqui uma análise comparando os dois valores de acurácia. A hipótese foi validada? O pré-processamento com YOLOv5 melhorou, piorou ou não alterou a performance? Discuta os possíveis motivos para o resultado obtido. Por exemplo, se a acurácia melhorou, foi porque o classificador pôde focar em características mais relevantes. Se piorou, pode ser que o recorte do YOLOv5, por ser uma `bounding box`, ainda continha ruído ou, em alguns casos, cortou partes importantes do objeto.

    **Decisão de Engenharia:**
    * Com base nos resultados, a abordagem [ESCOLHER A MELHOR] é recomendada para esta tarefa. A [melhora/piora] na acurácia [justifica/não justifica] a complexidade adicional do pipeline de duas etapas.
    ```

### **Tarefa 3.3: Finalização e Entrega**

  * **Ação (Usuário):**
    1.  **Desenhe o Diagrama de Arquitetura:** Use uma ferramenta como draw.io ou PowerPoint para criar um diagrama visual que mostre o fluxo de dados para a "Abordagem 2" (Imagem -\> Modelo YOLOv5 -\> Imagem Recortada -\> Modelo Classificador -\> Predição). Insira a imagem no seu notebook.
    2.  **Grave o Vídeo:** Grave um vídeo de até 5 minutos explicando o processo, as duas abordagens, mostrando os resultados e explicando suas conclusões.
    3.  **Atualize o GitHub:** Faça o commit final do notebook e adicione o link do vídeo ao seu README principal.
    4.  **Submeta o Projeto.**