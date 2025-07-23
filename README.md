# **Trabalho da Unidade III – Visão Computacional 2025.1**

**Projeto:** Sistema de Classificação de Danos em Veículos Utilizando YOLO  
**Autor:** Gabriel Rossine Paulino de Souza  
**Repositório:** [GitHub](https://github.com/Kirishimya/VehicleDamageCls)

---

## **1. Introdução**

A avaliação de danos em veículos é um processo fundamental em setores como seguradoras, oficinas mecânicas e locadoras. Tradicionalmente, essa análise é realizada manualmente, o que torna o processo lento, subjetivo e caro. O objetivo deste trabalho foi automatizar essa tarefa por meio de visão computacional, classificando imagens de veículos em **seis tipos de avarias**:

- **crack** (rachadura)  
- **dent** (amassado)  
- **glass shatter** (vidro quebrado)  
- **lamp broken** (farol danificado)  
- **scratch** (arranhão)  
- **tire flat** (pneu furado)  

Foi utilizado um modelo da família YOLO (You Only Look Once), pré‑treinado e ajustado para reconhecer esses padrões de danos.

---

## **2. Desenvolvimento e Técnicas Utilizadas**

### 2.1. Dataset

Foi utilizado um conjunto de dados público do Kaggle, contendo milhares de imagens rotuladas nas 6 classes mencionadas. A divisão foi feita com a biblioteca `split-folders`, resultando em **80% para treinamento** e **20% para validação**.

### 2.2. Modelo de Classificação

O modelo escolhido foi o **YOLOv8**, da biblioteca Ultralytics. Para acelerar o desenvolvimento, foi adotada a técnica de **Transfer Learning**, utilizando pesos pré‑treinados no ImageNet. Em seguida, o modelo foi refinado para a tarefa específica, com treinamento por **10 épocas**.

### 2.3. Ambiente e Treinamento

- **Ambiente:** Google Colab (com GPU)  
- **Linguagem:** Python 3  
- **Frameworks:** PyTorch, Ultralytics YOLO  
- **Notebook:** `CVAv3.ipynb`

---

## **3. Resultados**

### 3.1. Métricas de Desempenho

O modelo alcançou uma **acurácia de 97,5%** no conjunto de validação:

all 0.975 1
Speed: 0.0ms preprocess, 22.3ms inference, 0.0ms loss, 0.0ms postprocess per image

### 3.2. Matriz de Confusão

![Matriz de Confusão Normalizada](AV3/validation_results/vehicle_damage_val2/confusion_matrix_normalized.png)

**Análise da matriz:**

| Classe real       | Crack | Dent | GS    | LB    | Scratch | TF    |
|-------------------|:-----:|:----:|:-----:|:-----:|:-------:|:-----:|
| **crack**         | 1.00  |  —   |  —    | 0.01  |   —     |   —   |
| **dent**          |  —    | 0.97 |  —    | 0.01  |  0.02   |   —   |
| **glass shatter** |  —    |  —   | 0.99  |  —    |   —     |   —   |
| **lamp broken**   |  —    | 0.01 |  —    | 0.96  |  0.01   |   —   |
| **scratch**       | 0.02  | 0.01 | 0.02  |  —    |  0.97   | 0.02  |
| **tire flat**     | 0.01  |  —   |  —    |  —    |   —     | 0.98  |

O modelo apresentou excelente desempenho, especialmente em _crack_ e _glass shatter_. Os poucos erros ocorreram em casos limítrofes, como amassados leves confundidos com arranhões ou faróis levemente trincados classificados incorretamente.

### 3.3. Análise Qualitativa

Abaixo estão comparações visuais entre as **previsões do modelo** e os **rótulos reais**:

#### Lote de Validação 0

| Rótulos Reais                         | Previsões do Modelo                        |
|---------------------------------------|--------------------------------------------|
| ![Labels0](AV3/validation_results/vehicle_damage_val2/val_batch0_labels.jpg) | ![Pred0](AV3/validation_results/vehicle_damage_val2/val_batch0_pred.jpg) |

- **Acertos notáveis:** danos evidentes como _glass shatter_, _scratch_ e _tire flat_ foram classificados corretamente.  
- **Erros observados:** um pequeno amassado foi rotulado como _glass shatter_; uma rachadura leve em farol foi entendida como _lamp broken_.

#### Lote de Validação 1

| Rótulos Reais                         | Previsões do Modelo                        |
|---------------------------------------|--------------------------------------------|
| ![Labels1](AV3/validation_results/vehicle_damage_val2/val_batch1_labels.jpg) | ![Pred1](AV3/validation_results/vehicle_damage_val2/val_batch1_pred.jpg) |

- **Acertos:** danos mais nítidos e bem definidos, como arranhões profundos ou pneus murchos.  
- **Erros:** um farol rachado foi confundido com _glass shatter_; uma área deformada com reflexo foi classificada com menor confiança.

---

## **4. Conclusão**

Este projeto demonstrou que a aplicação de modelos YOLOv8 em visão computacional pode automatizar de forma confiável a classificação de danos veiculares. A acurácia de **97,5%** comprova o potencial da abordagem.

### Possíveis Extensões:

1. **Detecção com bounding boxes:** localizar o dano na imagem, além de classificá-lo.  
2. **Classificação de severidade:** por exemplo, dano leve, moderado ou grave.  
3. **Deploy:** integrar o modelo a um sistema web ou app móvel para uso prático por seguradoras ou oficinas.

---
