<div align="center">

# 🎨 Neural Style Transfer with VGG-19

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=flat-square&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-VGG--19-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)
![Gradio](https://img.shields.io/badge/UI-Gradio-F97316?style=flat-square)

[Setup](#setup) · [CLI](#run-a-transfer) · [Browser UI](#use-the-browser-interface) · [Layout](#project-layout)

</div>

---

Create a new image that retains the structure of a content image while matching the textures, colours, and visual patterns of a style image.

The implementation uses an ImageNet-pretrained VGG-19 CNN as a fixed feature extractor:

- **Content loss:** mean-squared error at `conv4_2`, which preserves the main spatial arrangement.
- **Style loss:** mean-squared error between normalized Gram matrices at `conv1_1` through `conv5_1`.
- **Optimization:** gradient descent updates the generated image itself; VGG-19 weights remain frozen.

## Setup

Use Python 3.10 or newer. A fresh virtual environment is recommended. Install the PyTorch build appropriate for your CPU or GPU from the [PyTorch installer](https://pytorch.org/get-started/locally/), then install this project:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -e .[dev]
```

The first transfer downloads the pretrained VGG-19 weights (about 550 MB) into PyTorch's model cache.

## Run a transfer

```powershell
style-transfer `
  --content .\images\content.jpg `
  --style .\images\style.jpg `
  --output .\outputs\styled.png `
  --steps 500 `
  --image-size 512
```

Or run without installing a console command:

```powershell
python -m neural_style_transfer --content .\images\content.jpg --style .\images\style.jpg
```

The tool automatically uses CUDA when it is available. On a CPU, start with `--image-size 256 --steps 200`; a GPU can comfortably use higher values. Use `--help` to see every control, including content/style balance, learning rate, and device selection.

## Use the browser interface

Start the local interface, then open the address it prints (normally `http://127.0.0.1:7860`) in a browser:

```powershell
style-transfer-ui
```

It accepts one content image and one style image, displays progress during optimization, and lets you save the generated PNG directly from the result panel. The interface stays local to your computer.

## Project layout

```text
src/neural_style_transfer/
  core.py       # VGG feature extraction, Gram matrices, and optimization
  cli.py        # command-line interface and image I/O
  frontend.py   # local Gradio browser interface
tests/          # small mathematical and configuration checks
```
