<p align="center">
  <img src="https://avatars.githubusercontent.com/u/200675744?s=400&u=84a3652eb38c28a5f5af154f31b5dbf3a287ea95" alt="DFloat11" width="12%"/>
  <img src="assets/brand.png" width="50%">
</p>

<p align="center">
  <a href="https://vectorspacelab.github.io/OmniGen2"><img src="https://img.shields.io/badge/Project%20Page-OmniGen2-yellow" alt="project page"></a>
  <a href="https://huggingface.co/DFloat11/OmniGen2-transformer-DF11"><img src="https://img.shields.io/badge/Model-🤗-yellow" alt="transformer model"></a>
  <a href="https://huggingface.co/DFloat11/OmniGen2-mllm-DF11"><img src="https://img.shields.io/badge/Model-🤗-yellow" alt="mllm model"></a>
</p>

# DFloat11 + OmniGen2

This is a **DFloat11 losslessly compressed** version of the original `OmniGen2` model. It reduces model size by **32%** compared to the original BFloat16 model, while maintaining **bit-identical outputs** and supporting **efficient GPU inference**.

🔥🔥🔥 **Thanks to DFloat11 compression, OmniGen2 can now run smoothly on a single 16GB GPU without any quality loss.** 🔥🔥🔥

We apply **Huffman coding** to losslessly compress the exponent bits of BFloat16 model weights, which are highly compressible (their 8 bits carry only ~2.6 bits of actual information). To enable fast inference, we implement a highly efficient CUDA kernel that performs on-the-fly weight decompression directly on the GPU.

The result is a model that is **~32% smaller**, delivers **bit-identical outputs**, and achieves performance **comparable to the original** BFloat16 model.

Learn more in our [research paper](https://arxiv.org/abs/2504.11651).

## 📊 Performance Comparison

| Metric                                          | OmniGen2 (BFloat16) | OmniGen2 (DFloat11) |
| ----------------------------------------------- | ------------------- | ------------------- |
| Model Size                                      | 16.23 GB            | 11.11 GB            |
| Peak GPU Memory<br>(1024×1024 image generation) | 18.41 GB            | 14.36 GB            |
| Generation Time<br>(A100 GPU)                   | 25 seconds          | 27 seconds          |

## 🚀 Quick Start

Requires a CUDA-compatible GPU with at least 16GB of VRAM.

### 🛠️ Environment Setup

#### ✅ Recommended Setup

```bash
# 1. Clone the repo
git clone https://github.com/LeanModels/OmniGen2-DFloat11.git
cd OmniGen2-DFloat11

# 2. (Optional) Create a clean Python environment
conda create -n omnigen2 python=3.11
conda activate omnigen2

# 3. Install dependencies
# 3.1 Install PyTorch (choose correct CUDA version)
pip install torch==2.6.0 torchvision --extra-index-url https://download.pytorch.org/whl/cu124

# 3.2 Install other required packages
pip install -r requirements.txt

# Note: Version 2.7.4.post1 is specified for compatibility with CUDA 12.4.
# Feel free to use a newer version if you use CUDA 12.6 or they fixed this compatibility issue.
# OmniGen2 runs even without flash-attn, though we recommend install it for best performance.
pip install flash-attn==2.7.4.post1 --no-build-isolation
```

#### 🌏 For users in Mainland China

```bash
# Install PyTorch from a domestic mirror
pip install torch==2.6.0 torchvision --index-url https://mirror.sjtu.edu.cn/pytorch-wheels/cu124

# Install other dependencies from Tsinghua mirror
pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple

# Note: Version 2.7.4.post1 is specified for compatibility with CUDA 12.4.
# Feel free to use a newer version if you use CUDA 12.6 or they fixed this compatibility issue.
# OmniGen2 runs even without flash-attn, though we recommend install it for best performance.
pip install flash-attn==2.7.4.post1 --no-build-isolation -i https://pypi.tuna.tsinghua.edu.cn/simple
```

---

### 🧪 Run Examples

The following examples will automatically download the **DFloat11 OmniGen2** model, and use the GPU to generate/edit images or generate text.

```bash
# Visual Understanding
bash example_understanding.sh

# Text-to-image generation
bash example_t2i.sh

# Instruction-guided image editing
bash example_edit.sh

# In-context generation
bash example_in_context_generation.sh
```

**Gradio Demo**:

```bash
# for only generating image
pip install gradio
python app.py
# Optional: Share demo with public link (You need to be able to access huggingface)
python app.py --share

# for generating image or text
pip install gradio
python app_chat.py
```

## Learn More About DFloat11

* **Paper**: [70% Size, 100% Accuracy: Lossless LLM Compression for Efficient GPU Inference via Dynamic-Length Float](https://arxiv.org/abs/2504.11651)
* **GitHub**: [https://github.com/LeanModels/DFloat11](https://github.com/LeanModels/DFloat11)
* **HuggingFace**: [https://huggingface.co/DFloat11](https://huggingface.co/DFloat11)

## OmniGen2 Introduction

**OmniGen2** is a powerful and efficient generative model. Unlike OmniGen v1, OmniGen2 features two distinct decoding pathways for text and image modalities, utilizing unshared parameters and a decoupled image tokenizer. OmniGen2 has competitive performance across four primary capabilities:

- **Visual Understanding**: Inherits the robust ability to interpret and analyze image content from its Qwen-VL-2.5 foundation.
- **Text-to-Image Generation**: Creates high-fidelity and aesthetically pleasing images from textual prompts.
- **Instruction-guided Image Editing**: Executes complex, instruction-based image modifications with high precision, achieving state-of-the-art performance among open-source models.
- **In-context Generation**: A versatile capability to process and flexibly combine diverse inputs—including humans, reference objects, and scenes—to produce novel and coherent visual outputs.

As an open-source project, OmniGen2 provides a powerful yet resource-efficient foundation for researchers and developers exploring the frontiers of controllable and personalized generative AI.

<p align="center">
  <img src="assets/teaser.jpg" width="95%">
  <br>
  <em>Demonstrations.</em>
</p>

<p align="center">
  <img src="assets/examples_edit.png" width="95%">
  <br>
  <em>Demonstration of OmniGen2's image editing capabilities.</em>
</p>

<p align="center">
  <img src="assets/examples_subject.png" width="95%">
  <br>
  <em>Demonstration of OmniGen2's in-context generation capabilities.</em>
</p>
