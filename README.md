# Segmentação Semântica de Incêndios Florestais em Imagens Sentinel-2

Repositório com os códigos, experimentos e figuras dos **Ciclo 1** e **Ciclo 2** do meu trabalho de mestrado:

> **Avaliação de Funções de Perda e Arquiteturas de Redes Neurais na Segmentação Semântica de Incêndios Florestais em Imagens Sentinel-2**

Desenvolvido no Programa de Pós-Graduação em Ciência da Computação (PPGCO) da Universidade Federal de Uberlândia (UFU).

---

## 🎯 Problema do Mestrado

Incêndios florestais causam impactos ambientais, econômicos e sociais significativos. Monitorar e mapear áreas queimadas em grande escala depende cada vez mais de **Sensoriamento Remoto** e de métodos automáticos baseados em **Aprendizado Profundo**.

O objetivo central deste trabalho é:

> **Avaliar diferentes arquiteturas de redes neurais profundas e funções de perda na tarefa de segmentação semântica de incêndios florestais em imagens Sentinel-2, em um cenário altamente desbalanceado (poucos pixels de fogo vs. muitos pixels de não-fogo).**

Desafios principais:

- Forte **desbalanceamento de classes**;
- Presença de **ruídos** e variação espectral em áreas queimadas e não queimadas;
- Necessidade de capturar **regiões pequenas, alongadas e fragmentadas** de fogo, mantendo boa generalização.

---

## 🔁 Ciclo 1 e Ciclo 2 — Visão Geral

### 🔹 Ciclo 1 — Baseline com U-Net + VGG16

- Arquitetura: **U-Net** com encoder **VGG16** pré-treinado em ImageNet;
- Funções de perda (variações testadas):
  - **Binary Cross-Entropy (BCE)**;
  - **Dice Loss**;
- Estratégias:
  - Augmentations geométricos e fotométricos;
  - Avaliação em validação e teste com:
    - **IoU**, **F1-score**, **Precisão**, **Recall**;
  - Análise de matrizes de confusão e mapas de segmentação.

### 🔹 Ciclo 2 — Encoders modernos e técnicas avançadas

- Arquiteturas:
  - **U-Net + EfficientNetB3** (foco principal do ciclo);
  - Comparações com o encoder VGG16 do Ciclo 1;
- Funções de perda:
  - **Função de perda composta (BCE + Dice)**;
- Estratégias adicionais:
  - **Balanceamento** via undersampling da classe não-fogo;
  - Ajuste fino de **threshold** para binarização das saídas;
  - Comparação detalhada entre Ciclo 1 e Ciclo 2 tanto em métricas quanto em mapas de segmentação.

---

## 💾 Base de Dados

A base de dados é composta por **patches de imagens Sentinel-2** e suas respectivas **máscaras binárias** indicando presença (`1`) ou ausência (`0`) de fogo/área queimada.

- **Link para download da base**  
  👉 [Acessar base de dados](https://[substituir-pelo-link-real].com)

> ⚠️ Os dados **não** são versionados neste repositório. Eles devem ser baixados pelo link acima e organizados localmente na estrutura indicada abaixo.

## 📌 Principais Achados

Alguns resultados consolidados dos ciclos experimentais (sem valores numéricos específicos):

- A combinação **U-Net + EfficientNetB3** com **função de perda composta (BCE + Dice)**, no **Ciclo 2**, apresentou **desempenho superior** ao baseline do **Ciclo 1 (U-Net + VGG16)** em métricas como **IoU** e **F1-score**, especialmente para a classe fogo.
- O uso de **balanceamento** (por exemplo, undersampling da classe não-fogo) reduziu o viés do modelo em favor da classe majoritária, melhorando a capacidade de detecção das regiões queimadas.
- As curvas de treino e validação dos modelos do **Ciclo 2** mostraram **maior estabilidade** e **melhor convergência** em comparação ao Ciclo 1, sugerindo uma otimização mais robusta da função de perda.
- A análise qualitativa dos mapas de segmentação indicou que:
  - O modelo com **EfficientNetB3** preserva melhor **pequenas manchas de fogo e contornos irregulares**, reduzindo a fragmentação das regiões queimadas;
  - Ainda ocorrem falsos positivos em áreas com padrões espectrais semelhantes a queimadas (solo exposto, sombras, determinadas áreas agrícolas), mas em menor intensidade em relação ao baseline;
  - Há uma relação mais equilibrada entre **sensibilidade** (detectar fogo) e **especificidade** (evitar classificar não-fogo como fogo), sobretudo após o ajuste de limiar (threshold) obtido por varredura.

> Os valores numéricos detalhados (IoU, F1, Precisão, Recall, etc.) podem ser consultados nos arquivos `.csv` em:
> - `experiments/cycle1/metrics/`
> - `experiments/cycle2/metrics/`

