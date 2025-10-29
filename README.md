# CellViT

**2025-10-29:** This repository contains PanNuke training and evaluation codebase of [CellViT](https://github.com/TIO-IKIM/CellViT). The original codebase can be found under [```CellViT-main-05097e1```](https://github.com/sumeromer/CellViT/tree/CellViT-main-05097e1) branch. I refactored to the current PyTorch and Python libraries for a better benchmarking by re-running CellViT training for cell segmentation.


## CellViT: Nuclei segmentation on PanNuke dataset
Cell segmentation training is done on the official PanNuke data splits:
- Training: Fold 1; Validation: Fold 2; Testing: Fold 3
- Training: Fold 2; Validation: Fold 1; Testing: Fold 3
- Training: Fold 3; Validation: Fold 2; Testing: Fold 1

### Prepare Python environment

```bash
micromamba env create --file environment.yml
micromamba activate CellSeg
```

### Download model weights to initialize encoder
First, download the model weights to initialize the encoder.

> *"For the pre-trained ViT256-model, we utilized the ViT-S checkpoint [1] provided by Chen et al. (2022). As for the SAM-B, SAM-L, and SAM-H models, we use the encoder backbones of each final training stage of SAM (Kirillov et al., 2023), published on GitHub. [2]"*. 
[1] https://github.com/mahmoodlab/HIPT  
[2] https://github.com/facebookresearch/segment-anything

To run ViT256 (ViT-S model with approx. 21 million trainable parameters), the respective model weights must be under the following path: ```./models/pretrained/ViT-256/vit256_small_dino.pth```

### Training

```bash
python cell_segmentation/run_cellvit.py  \
    --gpu 1 \
    --config ./configs/Pannuke-ViT256/config_fold1.yaml
```

### Inference 

**PanNuke Test Set:**
The following script calculates the nucleus detection and segmentation metrics reported in CellViT paper and makes visualizations on the entire test partition.

```bash
  python cell_segmentation/inference/inference_cellvit_experiment_pannuke.py \
      --run_dir $LOG_DIR \
      --checkpoint_name model_best.pth \
      --gpu 1 \
      --magnification 40 \
      --plots
```

**On WSI images:**  
TODO


## Citation:
```
@article{CellViT,
    title = {CellViT: Vision Transformers for precise cell segmentation and classification},
    journal = {Medical Image Analysis},
    volume = {94},
    pages = {103143},
    year = {2024},
    issn = {1361-8415},
    doi = {10.1016/j.media.2024.103143},
    url = {https://www.sciencedirect.com/science/article/pii/S1361841524000689},
    author = {Fabian Hörst and Moritz Rempe and Lukas Heine and Constantin Seibold and Julius Keyl and Giulia Baldini and Selma Ugurel and Jens Siveke and Barbara Grünwald and Jan Egger and Jens Kleesiek},
}
```

**Github repository:**  
https://github.com/TIO-IKIM/CellViT/tree/main






