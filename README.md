# CNP2P: Chinese Painting-to-Poetry Dataset

**CNP2P** (Chinese Painting-to-Poetry) is a benchmark dataset for generating classical Chinese poetry from traditional Chinese paintings. It contains **6,484 annotated samples** — **5,837 for training** and **647 for testing** — each pairing a painting image with its title, dynasty, artist, main visual elements, and artistic conception (意境).

The dataset accompanies the paper:

> **Painting Poetry Generation: A Benchmark Dataset and Feedback Framework for Cross-Modal Alignment**
> Senqi Yang, Dongyu Zhang, Haoxi Yang, Jiaying Liu, Ciyuan Peng, Mingshuo Pan, Wei Guo, Haojia Li, Hongfei Lin, Feng Xia
> *Expert Systems with Applications* (accepted)

## Data access

The complete dataset — all 6,484 images and the annotation files, about **5.14 GB** — is hosted on Hugging Face:

**<https://huggingface.co/datasets/senqiY/CNP2P>**

Because of GitHub's file-size and storage limits, the data is not mirrored in this repository.

### Repository layout on Hugging Face

```
CNP2P/
├── train/
│   ├── 1.jpg
│   ├── 5.jpg
│   └── ...                # 5,837 images
├── test/
│   ├── 2.jpg
│   └── ...                # 647 images
├── train.json             # 5,837 records
└── test.json              # 647 records
```

### Download

```bash
huggingface-cli download senqiY/CNP2P --repo-type dataset --local-dir CNP2P
```

Or in Python:

```python
from huggingface_hub import snapshot_download

path = snapshot_download("senqiY/CNP2P", repo_type="dataset")
```

## Data format

Images are stored as `{split}/{ID}.jpg`, and the annotation files are JSON arrays. Each record contains:

| Field | Type | Description |
|---|---|---|
| `ID` | string | Sample identifier; the image is `{split}/{ID}.jpg`. |
| `Title` | string | Title of the painting. |
| `dynasty` | string | Historical period, given in Chinese (see note below). |
| `author` | string | Artist name. |
| `Main elements` | string | Main visually identifiable objects or scene components in the painting. |
| `Artistic conception` | string | Overall aesthetic impression, emotional orientation, and thematic meaning conveyed by the painting. |
| `Reference Poetry` | string | Reference poem, available in `test.json` only. |

The `dynasty` values appear in Chinese and include some variants, e.g. `宋` / `南宋`, `元` / `金`, `明` / `明朝`, `清`, and `近现代` / `现代` / `民国`. For the five-period statistics reported in the paper, variants are grouped as follows:

| Period | Samples | Share |
|---|---|---|
| Ming (明) | 2,829 | 43.63% |
| Qing (清) | 2,379 | 36.69% |
| Modern (近现代 / 现代 / 民国) | 698 | 10.76% |
| Song (宋 / 南宋) | 436 | 6.72% |
| Yuan (元 / 金) | 142 | 2.19% |
| **Total** | **6,484** | **100%** |

## Splits

| Split | Samples |
|---|---|
| Train | 5,837 |
| Test | 647 |
| **Total** | **6,484** |

## Loading example

```python
import json
from pathlib import Path

from huggingface_hub import snapshot_download

root = Path(snapshot_download("senqiY/CNP2P", repo_type="dataset"))
records = json.loads((root / "train.json").read_text(encoding="utf-8"))
sample = records[0]

image_path = root / "train" / f"{sample['ID']}.jpg"
print(image_path)
print(sample["Title"], sample["dynasty"], sample["author"])
print(sample["Main elements"])
print(sample["Artistic conception"])
```

## Licensing

- **Annotations and metadata** (the JSON files) are released under the **Creative Commons Attribution-NonCommercial 4.0 International License (CC BY-NC 4.0)**: <https://creativecommons.org/licenses/by-nc/4.0/>.
- **Painting images** are reproduced from the public websites of the source museums and remain the property of their respective institutions. They are provided **solely for non-commercial academic research**, subject to the image-use policies of the source museums. Users are responsible for complying with those policies and for obtaining any additional permissions required for other uses.

If you believe that any content in this dataset infringes your rights, please contact the authors and the material will be removed.

## Citation

If you use CNP2P, please cite:

```bibtex
@article{yang2026cnp2p,
  title   = {Painting Poetry Generation: A Benchmark Dataset and Feedback Framework for Cross-Modal Alignment},
  author  = {Yang, Senqi and Zhang, Dongyu and Yang, Haoxi and Liu, Jiaying and Peng, Ciyuan and Pan, Mingshuo and Guo, Wei and Li, Haojia and Lin, Hongfei and Xia, Feng},
  journal = {Expert Systems with Applications},
  year    = {2026},
  note    = {Accepted}
}
```
