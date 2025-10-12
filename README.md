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
- Pipeline completo: coleta proprietária, rotulagem no Make Sense AI, treinamento em Google Colab, avaliação quantitativa, validação qualitativa e documentação executiva.
- Três experimentos controlados (YOLOv5s 30 épocas, YOLOv5s 60 épocas, YOLOv5m 60 épocas) quantificam o trade-off entre acurácia e custo computacional.
- Gráficos comparativos, tabela executiva e análise textual evidenciam a superioridade do modelo `YOLOv5m` e os impactos de governança de dados e deploy em edge.
- Discussão aprofundada sobre limitações de bounding boxes para objetos de geometria complexa e roadmap para evoluir o projeto (segmentação, hiperparâmetros, compressão).

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
- Execute sequencialmente: clonagem do YOLOv5, instalação de dependências, montagem do dataset, treinamentos (Exp_30_Epocas, Exp_60_Epocas, Exp_60_Epocas_Medium) e inferências.
- Monitore resultados em `runs/train/Exp_30_Epocas`, `runs/train/Exp_60_Epocas`, `runs/train/Exp_60_Epocas_Medium` e `runs/detect/Teste_Final` dentro do ambiente Colab.
- Exporte gráficos e tabela comparativa (`comparacao_curvas_aprendizado.png`, `comparativo_final_barras.png`, `comparacao_curvas_de_perda.png`, `tabela_comparativa_final.png`) para o Drive ao final da sessão.

## 🗃 Histórico de lançamentos

- 2025-10-10: Entrega oficial da POC com documentação completa e comparativo de experimentos.

## 📋 Licença

Este repositório segue o modelo [MODELO GIT FIAP](https://github.com/agodoi/template) licenciado sob [Creative Commons Attribution 4.0 International](http://creativecommons.org/licenses/by/4.0/?ref=chooser-v1).
