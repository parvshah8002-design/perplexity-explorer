# Perplexity Explorer

An interactive, visual exploration of how language models evaluate text, compute token probabilities, and measure perplexity.

🌐 **Live Demo**: [https://perplexity-explorer-steel.vercel.app](https://perplexity-explorer-steel.vercel.app)

---

## Overview

Perplexity is a fundamental metric in natural language processing (NLP) that measures how surprised a language model is by a sequence of text. A lower perplexity indicates that the model finds the sentence natural and predictable, while higher perplexity signals unusual phrasing or unexpected words.

This project is a single self-contained web app with **no backend, no frameworks, and no API keys**. It runs a real transformer language model (`DistilGPT-2`) directly inside the browser using [Transformers.js](https://huggingface.co/docs/transformers.js).

---

## Features

### 1. In-Browser Language Model Inference
- Loads **DistilGPT-2** with 8-bit quantization (`dtype: "q8"`) via Transformers.js in WebAssembly / WebGPU.
- Model downloads only on the first calculation and caches locally.
- Computes real forward-pass logits over the token sequence with start-of-text marker handling (`<|endoftext|>` / token ID `50256`).

### 2. Step-by-Step Perplexity Calculation
- Computes numerically stable softmax probabilities:
  $$\text{Softmax}(z_i) = \frac{e^{z_i - \max(z)}}{\sum_j e^{z_j - \max(z)}}$$
- Displays a clean breakdown table:
  - Token position `#`
  - Decoded text representation (spaces shown as `␣`, newlines as `↵`)
  - Token probability $P$
  - Model surprise $1/P$
- Shows the complete 3-step calculation:
  1. Multiply all $1/P$ values together.
  2. State $N$, the sequence length.
  3. Compute $\text{Perplexity} = (\text{Product})^{1/N} = \exp\left(\frac{1}{N} \sum -\ln P\right)$ using logarithms to avoid floating-point overflow.

### 3. Next-Token Predictions
- Visualizes the top 8 most probable next tokens predicted by the model at the final position.
- Interactive horizontal probability bars highlighting the #1 top candidate.

### 4. Interactive "Try It by Hand" Simulator
- Labeled *“Simulated, not from the model”*.
- Edit tokens and probabilities with dynamic row addition and removal.
- Quick presets:
  - **“I love Python”** ($0.5, 0.8, 0.2$) $\rightarrow$ Perplexity: **2.32**
  - **“All 1/8”** (eight tokens each at $0.125$) $\rightarrow$ Perplexity: **8.00**
  - **“One surprising word”** ($0.9, 0.9, 0.9, 0.01$) $\rightarrow$ Perplexity: **3.42**
- Updates the table and calculations reactively as you type.

---

## Getting Started

Because this application is self-contained in a single file (`index.html`), you can run it without any build tools or dependencies:

1. Clone this repository:
   ```bash
   git clone https://github.com/parvshah8002-design/perplexity-explorer.git
   cd perplexity-explorer
   ```
2. Open `index.html` in any modern web browser (Chrome, Edge, Safari, Firefox), or serve it locally:
   ```bash
   npx serve .
   # or
   python3 -m http.server 8000
   ```

---

## License

MIT License. Open source and free to use for educational and research purposes.
