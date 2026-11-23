# BanglaVisionNet: Lip-Landmark-Aided Bengali Visual Speech Recognition

Recognising spoken Bengali words from **silent video** using lip movements only.

> 📄 Published at **IEEE COMPAS 2025** (2nd International Conference on Computing, Applications and Systems), Kushtia, Bangladesh.
> *"Lip Landmark Aided Bengali Visual Speech Recognition leveraging Xception and Multi-GRU Architecture"*: Wahida Mahjabin, Sk. Md. Masudul Ahsan (KUET). The full paper PDF is included in this repository.

---

## Why this matters

Visual Speech Recognition (VSR), or lip reading, lets a computer understand speech when audio is missing, noisy or unusable. It is useful for:

- **Accessibility**: supporting people with hearing impairments
- **Noisy environments**: factories, crowds, vehicles
- **Silent communication and surveillance**: where audio is unavailable

Bengali has more than 270 million speakers, but before this work there was **no large-scale, visual-only Bengali word dataset**. Bengali is also hard to lip-read because many words look almost the same on the lips (for example *পানি / pani* "water" vs. *বাণী / bani* "words").

## What this project contributes

1. **BanglaVisionNet dataset**: a new word-level Bengali lip-reading dataset with roughly 9,000 video clips of **100 common Bengali words**, recorded by **41 volunteers** (23 male, 18 female, aged 14–52) on their own phone cameras at home. Every clip was manually checked for lighting, angle and clear pronunciation.
2. **Lip-landmark-guided feature extraction**: instead of feeding whole face images, the pipeline finds the mouth, crops it, and marks **20 key points on the upper and lower lips** (with Dlib). This directs the model's attention to how the lips move, and lowers the computing cost.
3. **Hybrid deep-learning model**: a 3D CNN captures short-term motion, an **Xception** backbone extracts visual features efficiently, and a **Bidirectional GRU + GRU** stack models how the lip shape changes over the word.

## How it works

```
Video of a spoken word
   → extract 30 key frames
   → detect face, crop mouth region, convert to grayscale
   → mark 20 lip landmarks on every frame
   → 3D CNN  →  Xception  →  BiGRU + GRU
   → predicted Bengali word (1 of 100)
```

## Results (from the paper)

| Setting | Result |
|---|---|
| BanglaVisionNet, 100 words (75/15/10 split) | **87% accuracy**, 89% F1 |
| LRW (English benchmark, 500 words) | **88% accuracy** |
| Unseen speakers (10 words) | 63% accuracy |

The paper also compares four backbones (ResNet18, Xception, ResNeXt101, VGG16) on three input types: raw frames, landmark-marked frames, and marked frames plus landmark coordinates. Xception with landmark-marked frames performed best on both datasets.

## Demo

A small **Gradio web demo** (in `finalruns44ClassGradio.ipynb`) lets you upload a short video of a spoken word. It runs the full preprocessing pipeline and returns the recognised Bengali word.

<!-- Add a screenshot or GIF of the Gradio demo here -->

## What's in this repository

| File | Purpose |
|---|---|
| `100ClassLastRun.ipynb` | Main training and evaluation on all 100 Bengali words |
| `SpeakerIndependentNew67.ipynb` | Speaker-independent experiment (testing on people the model never saw) |
| `lrwdataset.ipynb` | Training and evaluation on the English LRW benchmark |
| `InputOutputTesting.ipynb` | Preprocessing pipeline: frame extraction, mouth crop, Dlib lip landmarks |
| `finalruns44ClassGradio.ipynb` | Interactive Gradio demo |
| `*.pdf` | The published paper |

## Tech stack

Python · PyTorch · OpenCV · Dlib · scikit-learn · Gradio · Jupyter

## Dataset access

 available on request, here --> oro.wahida@gmail.com

## Citation

```bibtex
@inproceedings{mahjabin2025banglavsr,
  title     = {Lip Landmark Aided Bengali Visual Speech Recognition leveraging Xception and Multi-GRU Architecture},
  author    = {Mahjabin, Wahida and Ahsan, Sk. Md. Masudul},
  booktitle = {IEEE 2nd International Conference on Computing, Applications and Systems (COMPAS)},
  year      = {2025}
}
```
