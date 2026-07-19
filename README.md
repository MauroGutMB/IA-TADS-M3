# Detecção de Objetos Cotidianos com YOLOv5

**Projeto 20 — Inteligência Artificial**
**Autor:** Mauro Gutemberg — Instituto Federal do Piauí (IFPI), Campus Corrente

Detecção de objetos cotidianos em imagens variadas utilizando o **YOLOv5s** treinado sobre um **subconjunto de 10 classes do COCO-2017**, com investigação experimental obrigatória comparando duas resoluções de entrada: **416×416** e **640×640**.

## 📁 Estrutura do repositório

```
.
├── deteccao_yolov5_coco.ipynb   # Notebook com os dois experimentos completos
├── resultados/                  # Métricas e curvas geradas (após execução)
└── README.md
```

## 🎯 Sobre o projeto

- **Dataset:** COCO-2017 (subconjunto de 10 classes — ver justificativa abaixo)
- **Modelo:** YOLOv5s com *transfer learning* (pesos pré-treinados)
- **Investigação experimental:** impacto da resolução de entrada sobre acurácia e velocidade
- **Métricas:** mAP@0.5 · mAP@0.5:0.95 · Precisão · Recall · F1-score · FPS

### Classes utilizadas

`person` · `car` · `bicycle` · `dog` · `cat` · `chair` · `bottle` · `cup` · `laptop` · `cell phone`

**Justificativa:** o COCO completo (80 classes, 118k+ imagens) é inviável no ambiente de treinamento disponível. As 10 classes escolhidas (a) são objetos genuinamente cotidianos; (b) cobrem escalas grande, média e pequena — essencial para analisar o efeito da resolução de entrada; e (c) são bem representadas no COCO, garantindo amostras suficientes.

## ▶️ Como executar

### Opção 1 — Google Colab (recomendado)

1. Abra o notebook `deteccao_yolov5_coco.ipynb` no [Google Colab](https://colab.research.google.com);
2. Ative a GPU: **Ambiente de execução → Alterar tipo de ambiente de execução → GPU (T4)**;
3. Execute as células em ordem (**Ambiente de execução → Executar tudo**).

O notebook faz automaticamente: clone do YOLOv5, instalação das dependências, download do subconjunto do COCO via FiftyOne, treinamento dos dois experimentos, avaliação e geração das tabelas comparativas.

⏱️ Tempo estimado: **2–4 h** (50 épocas, GPU T4).

### Opção 2 — Máquina local com GPU

```bash
git clone https://github.com/MauroGutMB/yolov5-deteccao-objetos-cotidianos.git
cd yolov5-deteccao-objetos-cotidianos
pip install jupyter
jupyter notebook deteccao_yolov5_coco.ipynb
```

Requisitos: Python ≥ 3.8, GPU NVIDIA com CUDA, ~15 GB de disco (dataset + pesos + resultados).

> Ajuste `EXPORT_DIR` no notebook se não estiver no Colab (o padrão é `/content/coco10`).

## 🧪 Experimentos

| Experimento | Resolução | Épocas | Batch | Pesos iniciais |
|---|---|---|---|---|
| 1 | 416 × 416 | 50 | 16 | yolov5s.pt |
| 2 | 640 × 640 | 50 | 16 | yolov5s.pt |

A única variável manipulada é a resolução de entrada, garantindo comparação controlada. A avaliação usa o `val.py` oficial (NMS com IoU = 0.45) e o FPS é calculado a partir do tempo total por imagem (pré-processamento + inferência + NMS).

## 📊 Resultados

| Métrica | 416×416 | 640×640 |
|---|---|---|
| mAP@0.5 | 0.4288 | **0.4719** |
| mAP@0.5:0.95 | 0.2554 | **0.2849** |
| Precisão | 0.5954 | **0.6073** |
| Recall | 0.4099 | **0.4456** |
| F1-score | 0.4856 | **0.5140** |
| FPS | **159.2** | 141.6 |

A resolução 640×640 venceu em todas as métricas de acurácia (+10,1% relativo em mAP@0.5), com os maiores ganhos nas classes pequenas (*bottle*, *cup*) e de estrutura fina (*bicycle*). A resolução 416×416 foi apenas 12,4% mais rápida na GPU T4 — bem abaixo da razão teórica de custo (~2,37×), pois pré-processamento e NMS não dependem da resolução.

## 📚 Referências principais

- Redmon et al. *You Only Look Once: Unified, Real-Time Object Detection.* CVPR 2016.
- Jocher et al. *YOLOv5 by Ultralytics.* GitHub, 2020.
- Lin et al. *Microsoft COCO: Common Objects in Context.* ECCV 2014.
