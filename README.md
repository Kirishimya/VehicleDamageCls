# **Trabalho da Unidade III - Visão Computacional 2025.1**

**Projeto: Sistema de Classificação de Danos em Veículos Utilizando YOLO**

**Autor(es):** Gabriel Rossine Paulino de Souza
**Repositório do Projeto:** [Link para o GitHub](https://github.com/Kirishimya/VehicleDamageCls)

---

### **1. Introdução**

A avaliação de danos em veículos é um processo fundamental em diversos setores, como seguradoras, oficinas mecânicas e empresas de aluguel de carros. Tradicionalmente, essa tarefa é realizada manualmente por inspetores, o que pode ser um processo lento, subjetivo e caro. A automação dessa análise através da inteligência artificial pode trazer mais agilidade, objetividade e eficiência para o fluxo de trabalho.

O objetivo deste projeto foi desenvolver e avaliar um sistema de visão computacional capaz de classificar imagens de veículos em duas categorias principais: . Para isso, utilizamos um modelo de aprendizado profundo (Deep Learning) da família YOLO, que foi treinado para reconhecer padrões visuais característicos de avarias em automóveis.

---

### **2. Desenvolvimento e Técnicas Utilizadas**

O projeto foi desenvolvido inteiramente no ambiente Google Colab, aproveitando seus recursos de GPU para acelerar o treinamento do modelo. As principais tecnologias e metodologias empregadas estão descritas abaixo.

#### **2.1. Dataset**

Utilizamos um dataset público de imagens de veículos, obtido da plataforma Kaggle. O conjunto de dados continha milhares de imagens, previamente rotuladas em 6 classes:.

Para organizar os dados para o treinamento, utilizamos a biblioteca `split-folders` do Python, que dividiu o dataset automaticamente na seguinte proporção:


#### **2.2. Modelo de Classificação**

O núcleo do nosso sistema é um modelo de classificação da biblioteca **Ultralytics**, que implementa as mais recentes arquiteturas YOLO (You Only Look Once). Optamos por uma abordagem de **Aprendizagem por Transferência (Transfer Learning)**, utilizando um modelo YOLOv8 pré-treinado na vasta base de dados ImageNet.

Essa técnica consiste em aproveitar o "conhecimento" que o modelo já adquiriu sobre formas, texturas e objetos em geral e ajustá-lo (fine-tuning) para a nossa tarefa específica de identificar danos em carros. Isso acelera o treinamento e resulta em um modelo final com maior precisão.

#### **2.3. Ambiente e Treinamento**

* **Ambiente:** Google Colab
* **Linguagem:** Python 3
* **Frameworks:** PyTorch, Ultralytics YOLO
* **Treinamento:** O modelo foi treinado por 10 épocas (epochs), um número suficiente para que ele aprendesse as características das classes sem causar sobreajuste (overfitting). O processo de treinamento e validação pode ser consultado no notebook `CVAv3.ipynb`.

---

### **3. Resultados**

O modelo treinado apresentou um desempenho excelente na tarefa de classificação, demonstrando alta capacidade de generalização para imagens que não viu durante o treinamento.

#### **3.1. Métricas de Desempenho**

A acurácia final obtida no conjunto de dados de validação foi de **97.5%**. Isso significa que o modelo classificou corretamente 97.5% das imagens de veículos que nunca havia visto antes.

```
                   all      0.975          1
Speed: 0.0ms preprocess, 22.3ms inference, 0.0ms loss, 0.0ms postprocess per image
```

#### **3.2. Matriz de Confusão**

A matriz de confusão ilustra em detalhes o desempenho do classificador. A versão normalizada abaixo mostra a porcentagem de acertos para cada classe.

![Matriz de Confusão Normalizada](AV3/validation_results/vehicle_damage_val2/confusion_matrix_normalized.png)

**Análise da Matriz:**
* **Classe `damage` (com dano):** O modelo identificou corretamente **100%** das imagens que continham veículos danificados.
* **Classe `no_damage` (sem dano):** O modelo identificou corretamente **94.7%** das imagens de veículos sem avarias. Houve uma pequena taxa de erro (5.3%) onde o modelo classificou um carro sem danos como se tivesse.

Este resultado é muito positivo, pois demonstra que o sistema é extremamente confiável para sua finalidade principal: identificar veículos que de fato possuem danos.

#### **3.3. Análise Qualitativa**

Abaixo, comparamos as previsões do modelo (`pred`) com os rótulos verdadeiros (`labels`) em um lote de imagens de validação. A confiança da previsão é exibida acima de cada imagem.

**Lote de Validação 0:**
*Rótulos Reais:*
![Rótulos Reais - Lote 0](AV3/validation_results/vehicle_damage_val2/val_batch0_labels.jpg)

*Previsões do Modelo:*
![Previsões do Modelo - Lote 0](AV3/validation_results/vehicle_damage_val2/val_batch0_pred.jpg)

**Lote de Validação 1:**
*Rótulos Reais:*
![Rótulos Reais - Lote 1](AV3/validation_results/vehicle_damage_val2/val_batch1_labels.jpg)

*Previsões do Modelo:*
![Previsões do Modelo - Lote 1](AV3/validation_results/vehicle_damage_val2/val_batch1_pred.jpg)


Como observado nas imagens, o modelo realiza as classificações com altíssima confiança (próximo de 1.00), e os poucos erros não comprometem a eficácia geral da solução.

### **4. Conclusão**

Este projeto demonstrou com sucesso a aplicação de técnicas de visão computacional para resolver um problema do mundo real. O modelo desenvolvido, baseado na arquitetura YOLOv8, alcançou uma acurácia de 97.5%, provando ser uma ferramenta eficaz e confiável para a classificação automática de danos em veículos.

Como trabalhos futuros, sugerimos:
1.  **Evoluir para Detecção de Objetos:** Em vez de apenas classificar a imagem inteira, o modelo poderia ser treinado para desenhar uma caixa delimitadora (bounding box) exatamente sobre a área danificada.
2.  **Classificação de Severidade:** Expandir o número de classes para categorizar o nível do dano (ex: `leve`, `médio`, `grave`).
3.  **Implantação (Deploy):** Integrar o modelo a uma aplicação web ou móvel para uso prático por seguradoras e oficinas.
