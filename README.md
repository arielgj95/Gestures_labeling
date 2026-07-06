# Gestures Labeling

This repository contains a Jupyter notebook for labeling words in a sentence with
Symbolic and Deictic gestures. It accompanies the paper
["Labeling Sentences with Symbolic and Deictic Gestures via Semantic Similarity"](https://arxiv.org/abs/2407.02151),
accepted for IEEE RO-MAN 2024.

The main idea is simple: define a small set of gestures, describe the contexts in
which each gesture should appear, and compare those descriptions with short word
windows inside a target sentence. If a window is semantically close enough to a
gesture reference sentence, the words in that window receive the gesture label.

## Repository Contents

| Path | Description |
| --- | --- |
| `label_gestures.ipynb` | Main notebook with gesture definitions, semantic-similarity scoring, labeling algorithms, GUI-assisted human labeling utilities, and evaluation code. |
| `requirements.txt` | Python dependencies used by the notebook. |
| `CITATION.bib` | BibTeX entry for citing the related IEEE RO-MAN paper. |

Generated data files are intentionally not stored in this repository. The notebook
expects local pickle files for generated sentences, human labels, intermediate
similarity results, and final predictions.

## What The Notebook Implements

The notebook follows the workflow described in the paper:

1. Define 12 gesture classes: Greeting, Run, I apologize, I am exulting, I praise
   you, No, Yes, Indicate a place or object, Stop, I don't know, I beg you, and
   C'mon, it's late.
2. Define reference sentences for each gesture. These reference sentences describe
   the speech contexts in which a gesture is expected.
3. Use a Sentence-Transformers Cross-Encoder RoBERTa model to compute semantic
   similarity between reference sentences and candidate word windows.
4. Produce labels with the paper's main strategies:
   - a random/statistical baseline,
   - a fixed-window labeling algorithm,
   - a moving-window labeling algorithm.
5. Evaluate predictions against human labels with Average Precision, Intersection
   over Union, and average computation time.

The notebook also contains a Tkinter GUI used to collect human labels. That GUI is
useful for recreating the annotation process but is separate from the automatic
labeling algorithms.

## Setup

Use Python 3.8 or newer. A virtual environment is recommended:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

Then open `label_gestures.ipynb`.

The first model load may download the Cross-Encoder model from Hugging Face. The
notebook default is `cross-encoder/stsb-roberta-base`, matching the model family
reported in the paper's experimental setup. You can switch to a larger STS
Cross-Encoder if you want to compare quality and runtime.

## Data Files

The notebook uses local files such as generated sentence sets, human labels,
precomputed similarities, best-window estimates, and prediction outputs. The code
uses `path = ""` in many cells, which means those files are expected in the current
working directory unless you change `path`.

Do not commit private data or local outputs. Files such as `*.pkl`, `*.hkl`,
notebook checkpoints, and output folders are ignored by `.gitignore`.

Security note: pickle files can execute code when loaded. Only load pickle files
that you created yourself or fully trust.

## Alignment With The Paper

This repository is aligned with the public arXiv version of the paper:

- paper: Ariel Gjaci, Carmine Tommaso Recchiuto, and Antonio Sgorbissa,
  "Labeling Sentences with Symbolic and Deictic Gestures via Semantic Similarity";
- venue: IEEE RO-MAN 2024;
- arXiv: <https://arxiv.org/abs/2407.02151>;
- official conference site: <https://www.ro-man2024.org/>;
- implemented labels: 11 Symbolic gestures and 1 Deictic pointing gesture;
- implemented automatic methods: random/statistical baseline, fixed window, and
  moving window;
- main evaluation metrics: Average Precision, Intersection over Union, and average
  computation time.

An unfinished half-corpus experiment was present in the original notebook but was
not part of the paper workflow. It is now left as a disabled placeholder so the
notebook remains syntactically valid without implying that this experiment was
used in the reported results.

## Citation

If you use this notebook or build on the accompanying method, please cite:

```bibtex
@inproceedings{gjaci2024labeling,
  author = {Gjaci, Ariel and Recchiuto, Carmine Tommaso and Sgorbissa, Antonio},
  title = {Labeling Sentences with Symbolic and Deictic Gestures via Semantic Similarity},
  booktitle = {2024 33rd IEEE International Conference on Robot and Human Interactive Communication (RO-MAN)},
  year = {2024},
  note = {Accepted for publication; arXiv:2407.02151},
  url = {https://arxiv.org/abs/2407.02151}
}
```
