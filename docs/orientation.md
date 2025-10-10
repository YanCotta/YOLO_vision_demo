# Orientações da Fase 6

## 1) Descrição Rápida do Projeto

Para a Fase 6, vamos desenvolver uma rede neural. Além disso, haverá duas entregas extras denominadas **"Ir Além"**, que não valem nota.

Assim como na fase anterior, espera-se que os grupos se desafiem e aprendam ainda mais com essas duas entregas adicionais. Como recompensa pelas entregas "Ir Além", os grupos receberão gratificações (não notas) que serão explicadas ao longo das lives e neste documento.

---

## 2) Descrição Detalhada do Projeto

A FarmTech Solutions está expandindo os serviços de IA para além do agronegócio. A empresa agora presta serviços de IA nas áreas de saúde animal, segurança patrimonial de fazendas e residências, controle de acessos de funcionários, análise de documentos de diversas naturezas e visão computacional.

Nesta entrega, você integra o time de desenvolvedores da FarmTech e visita um cliente que deseja entender, na prática, como funciona um sistema de visão computacional.

**Objetivo:** criar um sistema de visão computacional usando YOLO que demonstre seu potencial e acurácia. Você é livre para escolher o cenário de imagens utilizado nas etapas de treinamento, validação e testes.

### Metas da Entrega 1

- Organizar um dataset com, no mínimo, 40 imagens de um objeto A e 40 imagens de um objeto B (diferente do A), totalizando 80 imagens.
- Reservar 32 imagens de cada objeto para treino, 4 para validação e 4 para testes.
- Armazenar as imagens no Google Drive pessoal ou do grupo, separando em pastas de treino, validação e teste.
- Realizar a rotulação das imagens de treinamento no site Make Sense AI.
- Salvar as anotações no Google Drive.
- Montar um notebook no Google Colab, conectado ao Google Drive, que realize treinamento, validação e teste, descrevendo em Markdown o passo a passo dessas três etapas.
- Executar ao menos duas simulações com quantidades diferentes de épocas (por exemplo, 30 e 60) e comparar acurácia, erro e desempenho.
- Apresentar conclusões sobre os resultados obtidos na validação e nos testes. Os resultados estarão disponíveis em `Results saved to yolov5/runs/detect/expX`, onde X incrementa a cada treino.
- Incluir prints das imagens de teste processadas pelo modelo para demonstrar o resultado ao cliente fictício da FarmTech Solutions.

### Entregáveis da Entrega 1

- Disponibilizar a solução em um novo repositório GitHub com o nome do grupo (1 a 5 pessoas ou solo) e enviar o link via portal FIAP. Um arquivo PDF pode ser utilizado para compartilhar o link.
- Não realizar novos commits após a data de entrega, para evitar classificação como entrega atrasada.
- Publicar no repositório o link para o notebook Jupyter, que será executado durante a correção. O notebook deve conter:
  - Células de código executadas, com Python otimizado e comentários quando necessário.
  - Células Markdown que organizem o relatório, expliquem os achados, destaquem pontos fortes e limitações do trabalho.
  - Nome do arquivo no formato `NomeCompleto_RM_pbl_fase6.ipynb` (ex.: `JoaoSantos_rm76332_pbl_fase6.ipynb`).
- Produzir um vídeo de até 5 minutos demonstrando o funcionamento da entrega, hospedá-lo no YouTube como **não listado** e incluir o link no README do GitHub.
- Desenvolver o README com documentação introdutória que direcione o leitor ao notebook Colab/Jupyter, onde está todo o passo a passo da solução e sua descrição completa. Não é necessário repetir o conteúdo do notebook no README, mas sim integrá-los de forma consistente.
- Manter os repositórios públicos para acesso da equipe FIAP, tomando cuidado para que os links não vazem ou sejam plagiados.
