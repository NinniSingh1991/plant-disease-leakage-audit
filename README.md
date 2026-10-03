# Result files: leakage, calibration and label efficiency in plant disease recognition

This repository holds the result files that accompany the article

> N. Singh, V. K. Gunjan, Rajkumar, M. K. Gupta, S. Kumar and G. Kumar, "Auditing near-duplicate leakage, calibration and label efficiency in plant disease recognition using a hybrid convolutional and gradient-boosted classifier," submitted to *IEEE Access*.

It contains the partition, the near-duplicate audit output, the field-transfer category mapping, the per-experiment result records and the test-set predictions from which the reported figures are computed. No images are redistributed; both corpora are public (see below).

## Contents

| Path | Description |
|---|---|
| `partition/plantvillage_partition.csv` | Split assignment (train, val or test) of every PlantVillage colour image, with a flag for the test images removed by the audit |
| `partition/removed_test_images.csv` | The 137 test images within Hamming distance 5 of a training image, removed before evaluation |
| `partition/fieldplant_category_mapping.csv` | Mapping of the seven FieldPlant categories to PlantVillage classes used in the field-transfer evaluation |
| `audit/leakage_audit_test.json` | Near-duplicate audit of the test split against the training split (counts at thresholds 0, 2, 5, 8 and 10; closest pairs) |
| `audit/leakage_audit_val.json` | The same audit for the validation split |
| `results/results_D_ft15.json` | Headline result of the fine-tuned hybrid pipeline on the audited test partition |
| `results/analyse_ft.json` | Per-class results, label-efficiency curves and diagnostic analyses |
| `results/arch_experiments.json` | Head and fusion experiments (weighted, stacked, hierarchical) and the 200-round control |
| `results/shap_analysis.json` | Exact tree-based Shapley attribution over the fused descriptor |
| `results/fieldplant_transfer.json` | Field-transfer evaluation on 2,320 FieldPlant leaves |
| `results/latency_1t.json`, `results/latency_12t.json` | Per-stage inference latency at batch size 1 with one and twelve CPU threads |
| `results/plantvillage_cpu_frozen.json` | Frozen-encoder (no fine-tuning) results for the fused and deep-only configurations |
| `results/results_D_ssl30_kaggle.json` | Self-supervised pretraining run; values were transcribed from the console log, as noted inside the file |
| `predictions/proba_fused_ft.npy`, `predictions/proba_deep_ft.npy` | Class-probability matrices (8,010 × 38) for the fused and deep-only configurations on the audited test partition |
| `predictions/te_y.npy` | True labels of the audited test partition, indexed as in `predictions/classes.json` |

Accuracy, macro-F1, expected calibration error, Brier score and the coverage–accuracy curve can be recomputed directly from the probability matrices and labels.

## Partition protocol

- Source: PlantVillage colour images (54,305 files, 38 classes).
- Split: stratified 70/15/15 per class. Within each class the file names are sorted, permuted with `numpy.random.default_rng(42)`, and divided in that order, with classes processed in sorted order.
- Audit: every image is reduced to a 64-bit difference hash; each test image is compared with the whole training split by multi-index hashing. Test images within Hamming distance 5 of a training image are removed (137 images, four byte-identical).
- Resulting sizes: 38,012 train, 8,146 validation and 8,010 test images.

## Datasets

- PlantVillage: S. P. Mohanty, D. P. Hughes and M. Salathé, "Using deep learning for image-based plant disease detection," *Front. Plant Sci.*, 2016, doi:10.3389/fpls.2016.01419. Available at https://github.com/spMohanty/PlantVillage-Dataset
- FieldPlant: E. Moupojou et al., "FieldPlant: A dataset of field plant images for plant disease detection and classification with deep learning," *IEEE Access*, vol. 11, pp. 35398–35410, 2023.

## License

The files in this repository are released under the Creative Commons Attribution 4.0 International licence (CC BY 4.0). The images themselves remain under the licences of their original datasets.
