# AfricanAlkanesML

This repository is designed to accompany the publication:

> Tweedy, R.R., Shi, S.C. & Uno, K.T. 2026. Supervised Machine Learning Approaches to African Plant Chemotaxonomy from *n*-Alkane Chain Length Abundances. *Organic Geochemistry*.
> DOI: [pending]

This repository contains the dataset, preprocessing code, and three distinct classifier notebooks used to test whether leaf-wax *n*-alkane chain-length distributions can distinguish woody from grassy vegetation across the African dataset compiled for this study.

---

## Repository Contents

    ├── test_train_split.ipynb          # Step 1 — Preprocessing, splitting, SMOTE augmentation, CLR transform
    ├── PCA.ipynb                       # Step 2a — Exploratory principal component analysis
    ├── LinearModels_and_SVM.ipynb      # Step 2b — Random Forests, Decision Tree, Linear/Non-linear Support Vector Machines, K-Nearest Neighbors, Gaussian Naive Bayes
    ├── DeterministicNeuralNetwork.ipynb # Step 2c — Deterministic neural network classifier
    ├── BayesianNeuralNetwork.ipynb     # Step 2d — Variational (Bayesian) neural network classifier
    ├── data/
    │   └── All_Africa_ML_dataset.csv   # Compiled plant n-alkane and metadata dataset
    ├── requirements.txt
    ├── LICENSE
    └── README.md

---

## Workflow Overview

Step 1 must be run first — it produces `test_train_df.csv`, the pre-treated (split, augmented, CLR-transformed) dataset that every other notebook loads. Steps 2a–2d are independent of one another: they each read `test_train_df.csv` and can be run in any order, in parallel, or individually, since none of them write outputs consumed by the others.

    Step 1 — test_train_split.ipynb
        Load All_Africa_ML_dataset.csv, label samples Woody/Grassy from `pft`,
        stratify a validation split, SMOTE-augment the minority (Grassy) class,
        filter synthetic samples to plausible composition space, and apply a
        centered log-ratio (CLR) transform to the alkane relative abundances.
        Output: test_train_df.csv
                 │
        ┌────────┼─────────────────┬─────────────────────┐
        ▼        ▼                 ▼                     ▼                    
    Step 2a  Step 2b            Step 2c                Step 2d
    PCA      Linear Models      Deterministic NN       Bayesian NN
    (explor- + SVM (RF, DT,     (feed-forward           (variational final layer,
     atory)   linear/RBF SVM,   PyTorch classifier)      MC-sampled predictive
              KNN, GNB)                                  uncertainty)

Each of the four downstream notebooks (2a–2d) independently reloads `test_train_df.csv`; none of them depend on each other's outputs.

---

## Installation

    pip install -r requirements.txt

`requirements.txt`:

    numpy>=1.23
    pandas>=1.5
    matplotlib>=3.6
    seaborn>=0.12
    scikit-learn>=1.1
    scipy>=1.9
    composition_stats>=2.0
    imbalanced-learn>=0.10
    torch>=2.0

All models are small and train in seconds (linear classifiers) to minutes (neural network's) on CPU; a GPU is not required. `torch` will use CUDA automatically if available.

---

## Reproducibility Notes

- Each ML/NN notebook carries an `output_file` variable in its parameter cell, intended for use with `papermill` if you want to batch-run a notebook multiple times (e.g., across sensitivity-test variants of `test_train_df.csv`) with different output names.

---

## Citation

If you use this code, please cite the manuscript above and the following software:

- Pedregosa, F. et al. (2011). Scikit-learn: Machine Learning in Python. *Journal of Machine Learning Research*, 12, 2825–2830.
- Chawla, N.V., Bowyer, K.W., Hall, L.O. & Kegelmeyer, W.P. (2002). SMOTE: Synthetic Minority Over-sampling Technique. *Journal of Artificial Intelligence Research*, 16, 321–357.
- Lemaître, G., Nogueira, F. & Aridas, C.K. (2017). Imbalanced-learn: A Python Toolbox to Tackle the Curse of Imbalanced Datasets in Machine Learning. Journal of Machine Learning Research, 18(17), 1–5.
- Paszke, A. et al. (2019). PyTorch: An Imperative Style, High-Performance Deep Learning Library. *Advances in Neural Information Processing Systems*, 32.
---

## License

This code is released under the MIT License. See LICENSE for details.
