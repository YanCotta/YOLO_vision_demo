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

- Prova de conceito de visão computacional para a FarmTech Solutions utilizando YOLOv5 para detectar `banana` e `fork`.
- A solução cobre coleta, rotulagem, treinamento, avaliação, inferência e documentação executiva com foco em decisões de negócio.
- Dois experimentos (30 vs 60 épocas) demonstram o impacto do tempo de treinamento nas métricas de mAP e na estabilidade do modelo.
- Discussão aprofundada sobre limitações de bounding boxes para objetos de geometria complexa e caminhos de evolução para segmentação.

- Vídeo explicativo (YouTube não listado): **adicionar link após publicação**

## 💽 Fontes de dados

- Dataset proprietário com 80 imagens (40 por classe) capturadas manualmente.
- Divisão estratificada: 64 treino, 8 validação, 8 teste (80/10/10).
- Rotulagem feita no Make Sense AI e armazenada no Google Drive junto com as imagens.

## 📁 Estrutura de pastas

- `docs/orientation.md`: enunciado oficial e checklist dos requisitos.
- `docs/report.md`: relatório executivo, metodologia e recomendações estratégicas.
- `notebooks/YanPimentelCotta_RM562836_fase6_cap1.ipynb`: notebook Colab com pipeline end-to-end.
- `assets/`: logotipos e materiais visuais de apoio.

## 🔧 Como executar o código

- Abra `notebooks/YanPimentelCotta_RM562836_fase6_cap1.ipynb` no Google Colab.
- Monte o Google Drive e ajuste os caminhos se necessário (`/content/drive/MyDrive/PBL6_Project_YOLO/`).
- Execute sequencialmente: clonagem do YOLOv5, instalação de dependências, montagem do dataset, treinamentos (30 e 60 épocas) e inferências.
- Monitore resultados em `runs/train/Exp_30_Epocas`, `runs/train/Exp_60_Epocas` e `runs/detect/Teste_Final` dentro do ambiente Colab.

## 🗃 Histórico de lançamentos

- 2025-10-10: Entrega oficial da POC com documentação completa e comparativo de experimentos.

## 📋 Licença

Este repositório segue o modelo [MODELO GIT FIAP](https://github.com/agodoi/template) licenciado sob [Creative Commons Attribution 4.0 International](http://creativecommons.org/licenses/by/4.0/?ref=chooser-v1).
