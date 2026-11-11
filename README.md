# Plant Disease Classification with Deep Learning

COMP6001 - Computer Vision and Multimodal Machine Learning - Final Project
Sabyasachi Kumar (A3197722)

This project classifies tomato, potato and apple leaf images (17 crop-disease classes) from the open-access
[PlantVillage dataset](https://github.com/digitalepidemiologylab/plantvillage_deeplearning_paper_dataset).
A custom convolutional neural network (CNN) is compared with classical machine learning (SVM, random forest),
transfer learning (ResNet18) and multi-modal methods that use image, text and metadata. A CNN is also implemented
from scratch in NumPy and verified against PyTorch.

## Repository contents

```
notebooks/
    dataset_analysis.ipynb            download data, multi-modal dataset, splits, analysis
    literature_review.ipynb           history, prior work, research gap, novelty, drawbacks, state of the art
    custom_cnn.ipynb                  custom CNN in PyTorch: preprocessing, features, training, evaluation
    benchmark_methods.ipynb           seven benchmark methods (classical, transfer learning, multi-modal)
    implementation_from_scratch.ipynb NumPy CNN from scratch, tests, complexity and limitations
    results_analysis.ipynb            metrics, confidence intervals, statistical tests, error analysis, findings
data/       created by the notebooks (images, splits.csv, cached features)  - not stored in git
models/     created by the notebooks (trained weights)
results/    created by the notebooks (figures, tables, predictions)
requirements.txt
README.md
```

## How to run

1. Install Python 3.10 or newer and `git` (check with `git --version`). On macOS use `python3` and `pip3`.
2. Create an environment and install the packages:
   ```
   python3 -m venv venv
   source venv/bin/activate          # Windows: venv\Scripts\activate
   pip install -r requirements.txt
   ```
3. Start Jupyter (or open the folder in VS Code) **from the repository root**, choose the `venv` kernel and run the
   notebooks in the order below, each with *Run All*.

| Order | Notebook | Needs | Approx. run time |
|---|---|---|---|
| 1 | `dataset_analysis.ipynb` | internet, git | 5 - 10 min |
| 2 | `custom_cnn.ipynb` | notebook 1 | 20 - 40 min |
| 3 | `benchmark_methods.ipynb` | notebook 1 (notebook 2 optional, for the comparison) | 30 - 45 min |
| 4 | `implementation_from_scratch.ipynb` | notebook 1 | 5 - 10 min |
| 5 | `results_analysis.ipynb` | notebooks 2, 3 and 4 (the predictions they save) | 1 - 2 min |
| - | `literature_review.ipynb` | nothing | seconds |

Use `QUICK_TEST = True` for an initial smoke test where the setting is available.
Before producing final results, set `QUICK_TEST = False` in `custom_cnn.ipynb`,
`benchmark_methods.ipynb`, and `implementation_from_scratch.ipynb`, then rerun
all three so their predictions use the same complete test split. Run
`results_analysis.ipynb` last. It reports a warning and intersects the available
prediction paths if model notebooks were run on different subsets; results from
that reduced intersection are not the final full-test comparison.

## Data

The data is not stored in this repository. `dataset_analysis.ipynb` downloads only the colour images of Tomato (10 classes),
Potato (3) and Apple (4) with a git sparse checkout: 23,483 images in `data/plantvillage/`. It also writes
`data/splits.csv`, the stratified 80/10/10 split used by every other notebook, so that all models are trained and tested
on identical images.

## Outputs

* `results/dataset_analysis/`, `results/custom_cnn/`, `results/benchmarks/`, `results/implementation/`, `results/literature_review/`, `results/analysis/` - figures and tables
* `results/predictions/` - test-set predictions and metrics of every model (columns `path`, `y_true`, `y_pred`)
* `models/` - trained weights (custom CNN, fine-tuned ResNet18, NumPy CNN)

## Troubleshooting

* **`python: command not found`** on macOS: use `python3`.
* **`data/splits.csv not found`**: run `dataset_analysis.ipynb` first.
* **`No prediction files found`** (results notebook): run the model notebooks first; they save their predictions in `results/predictions/`.
* **Different test subsets warning** (results notebook): rerun the custom CNN, benchmark, and from-scratch notebooks with `QUICK_TEST = False`, then rerun the results notebook.
* **Download stops part-way**: delete `_pv_tmp/` (and, if needed, `data/plantvillage/`) and run the notebook again.
* **Features look stale after changing the preprocessing**: delete `data/features/`; the benchmark notebook caches them.
* **Slow training**: use `QUICK_TEST = True`, set `EPOCHS = 5`, or use `IMG_SIZE = 128` in `custom_cnn.ipynb`.
* **Paths fail**: start Jupyter from the repository root, not from another folder.
* **ResNet18 weights cannot be downloaded**: the first run of `benchmark_methods.ipynb` needs internet access.

## Known limitations

PlantVillage images have uniform backgrounds, so high scores do not predict field performance; several photographs can
show the same leaf, so a random split may leak near-duplicates between sets; the text and metadata are class-level;
results come from a single split and seed. Each notebook ends with a section discussing its own limitations in detail.

## Reference

Mohanty, S. P., Hughes, D. P. & Salathe, M. 2016, Using deep learning for image-based plant disease detection,
*Frontiers in Plant Science*, vol. 7, article 1419, https://doi.org/10.3389/fpls.2016.01419
