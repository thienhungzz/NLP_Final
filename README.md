<div align="center">

<img src="https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
<img src="https://img.shields.io/badge/🤗_Transformers-4.45.0-FFD21E?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Gradio-5.5.0-FF7C00?style=for-the-badge&logo=gradio&logoColor=white"/>
<img src="https://img.shields.io/badge/FAISS-Vector_Store-00B4D8?style=for-the-badge"/>
<img src="https://img.shields.io/badge/QLoRA-Fine--tuning-8338EC?style=for-the-badge"/>

# 🎓 Hệ thống Hỏi Đáp Quy Chế TDTU

**Vietnamese QA System · RAG + LLM Fine-tuning · Đại học Tôn Đức Thắng**

---

> Dự án cuối kỳ môn **Nhập môn Xử lý Ngôn ngữ Tự nhiên (504045)**  
> Xây dựng hệ thống AI hỏi đáp thông minh về quy chế, quy định của Đại học Tôn Đức Thắng  
> sử dụng kỹ thuật **Retrieval-Augmented Generation (RAG)** kết hợp **Fine-tuning LLM**

</div>

---

## 👤 Tác giả

| Họ và Tên | MSSV |
|---|---|
| Trần Thiên Hưng | 52300109 |
| Hoàng Sinh Hùng | 52300106 |
| Lâm Ngọc Như Ý  | 52300171 |
---

## 📑 Mục lục

- [Tổng quan](#-tổng-quan)
- [Kiến trúc hệ thống](#-kiến-trúc-hệ-thống)
- [Tính năng](#-tính-năng)
- [Công nghệ sử dụng](#-công-nghệ-sử-dụng)
- [Cài đặt & Chạy](#-cài-đặt--chạy)
- [Workflow chi tiết](#-workflow-chi-tiết)
- [Kết quả đánh giá](#-kết-quả-đánh-giá)
- [Cấu trúc thư mục](#-cấu-trúc-thư-mục)
- [Dependencies](#-dependencies)

---

## 🎯 Tổng quan

Hệ thống AI có khả năng **trả lời chính xác** các câu hỏi về quy chế, quy định của Đại học Tôn Đức Thắng bằng cách kết hợp hai kỹ thuật mạnh mẽ:

- **RAG (Retrieval-Augmented Generation)** — truy xuất thông tin từ tài liệu gốc trước khi sinh câu trả lời
- **LLM Fine-tuning (QLoRA + RLHF)** — tối ưu hóa mô hình ngôn ngữ cho domain cụ thể

### Mục tiêu

| Tiêu chí | Mô tả |
|---|---|
| ✅ Độ chính xác cao | Thông tin trích xuất từ nguồn tài liệu chính thức của TDTU |
| ✅ Tính linh hoạt | Hỗ trợ đa dạng loại câu hỏi về quy chế |
| ✅ Giao diện thân thiện | Demo web với Gradio, so sánh các cấu hình real-time |
| ✅ Khả năng mở rộng | Pipeline hoàn chỉnh từ dữ liệu thô đến sản phẩm cuối |

---

## 🏗️ Kiến trúc hệ thống

```
┌─────────────────────────────────────────────────────────┐
│                   INPUT PIPELINE                        │
│                                                         │
│  📄 Tài liệu gốc (20 files .txt)                       │
│         ↓                                               │
│  🔄 Hybrid Agentic Chunking                             │
│         ↓                                               │
│  🤖 QA Generation  ←  DeepSeek API                     │
└────────────────────┬────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────┐
│                   MODEL PIPELINE                        │
│                                                         │
│  🔍 Vector Store (FAISS + BKAi Vietnamese Embedder)     │
│         ↓                                               │
│  🎯 RAG Pipeline                                        │
│         ↓                                               │
│  🔧 SFT Fine-tuning (QLoRA / Qwen2.5-3B)               │
│         ↓                                               │
│  🎪 RLHF — PPO + Reward Model  [optional]              │
└────────────────────┬────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────┐
│  🌐 Gradio Demo  ·  http://localhost:7860               │
└─────────────────────────────────────────────────────────┘
```

### Các cấu hình thực nghiệm

| Config | Mô tả | Ghi chú |
|:---:|---|---|
| **A** | Base model, không RAG | Baseline |
| **B** | Base model + RAG | |
| **C** | Fine-tuned model, không RAG | |
| **D** | Fine-tuned model + RAG | ⭐ Recommended |
| **E** | PPO (RLHF) + RAG | ⭐⭐ Best |

---

## ✨ Tính năng

- 🔍 **RAG Pipeline** — Tìm kiếm semantic và trích xuất thông tin từ kho tài liệu
- 🤖 **Fine-tuning LLM** — QLoRA fine-tune Qwen2.5-3B cho domain quy chế TDTU
- 🎯 **RLHF** — Cải thiện chất lượng câu trả lời qua phản hồi con người (PPO)
- 📊 **Đánh giá đa chiều** — BLEU, ROUGE-L, BERTScore, Recall@5
- 🎨 **Demo tương tác** — So sánh các cấu hình A/B/C/D/E real-time
- 📈 **Visualization** — Biểu đồ và thống kê kết quả đánh giá chi tiết

---

## 🛠️ Công nghệ sử dụng

### Core Stack

| Thành phần | Công nghệ |
|---|---|
| Language Model | Qwen2.5-3B-Instruct (4-bit quantized) |
| Fine-tuning | QLoRA (PEFT), PPO (TRL) |
| Embeddings | BKAi Vietnamese Bi-Encoder |
| Vector Store | FAISS IndexFlatIP |
| UI Framework | Gradio 5.5.0 |
| Language | Python 3.8+ |

### Thư viện chính

```
# LLM Fine-tuning
transformers==4.45.0 · peft==0.13.0 · trl==0.11.4
bitsandbytes==0.44.1 · accelerate==1.0.1 · datasets==3.1.0

# Embeddings & Retrieval
sentence-transformers==3.2.1 · faiss-cpu==1.8.0 · langchain==0.3.7

# Evaluation
sacrebleu==2.4.3 · rouge-score==0.1.2 · bert-score==0.3.13

# UI & Utils
gradio==5.5.0 · pymupdf>=1.24.0 · tqdm>=4.66.0
```

---

## 🚀 Cài đặt & Chạy

### Yêu cầu hệ thống

```
Python   ≥ 3.8
GPU      CUDA-compatible (khuyến nghị cho training)
RAM      ≥ 16 GB
Disk     ≥ 50 GB
```

### Bước 1 — Clone & cài đặt

```bash
git clone https://github.com/sigmaveol/TDTU-Vietnamese-QA-RAG.git
cd TDTU-Vietnamese-QA-RAG
pip install -r requirements.txt
```

### Bước 2 — Chuẩn bị dữ liệu & huấn luyện

Chạy các notebook theo thứ tự:

```bash
# 1. Chuẩn bị dữ liệu và chunking
jupyter notebook notebooks/01_setup_data_prep.ipynb

# 2. Sinh cặp QA tự động
jupyter notebook notebooks/02_qa_generation.ipynb

# 3. Xây dựng RAG pipeline
jupyter notebook notebooks/03_rag_pipeline.ipynb

# 4. SFT Fine-tuning với QLoRA
jupyter notebook notebooks/04_sft_training.ipynb

# 5. Đánh giá và thực nghiệm
jupyter notebook notebooks/05_experiments_eval.ipynb

# 6. RLHF — tùy chọn
jupyter notebook notebooks/06_rlhf_optional.ipynb
```

### Bước 3 — Chạy demo

```bash
python app.py
```

Truy cập **`http://localhost:7860`** để sử dụng giao diện web.

### Chạy trên Google Colab

1. Upload toàn bộ thư mục lên **Google Drive**
2. Mở từng notebook trong Colab và chạy theo thứ tự
3. Dùng `demo.launch(share=True)` để tạo public URL

---

## 🔄 Workflow chi tiết

### Phase 1 · Data Preparation

```
Load 20 file quy chế TDTU (.txt)
    → Hybrid Agentic Chunking (tạo chunks có ngữ nghĩa)
    → QA Generation qua DeepSeek API (cặp câu hỏi - trả lời)
```

### Phase 2 · Model Development

```
Xây dựng Vector Store (FAISS + Vietnamese Embeddings)
    → Supervised Fine-tuning: QLoRA trên Qwen2.5-3B
    → RLHF [Optional]: PPO training với Reward Model
```

### Phase 3 · Evaluation & Demo

```
Multi-config Experiments (so sánh cấu hình A → E)
    → Tính toán Metrics: BLEU, ROUGE, BERTScore, Recall@5
    → Gradio Demo: so sánh real-time giữa các cấu hình
```

---

## 📊 Kết quả đánh giá

### Automatic Metrics (test set)

| Config | BLEU ↑ | ROUGE-L ↑ | BERTScore ↑ | Recall@5 ↑ |
|:---:|:---:|:---:|:---:|:---:|
| A — Base, no RAG | 20.37 | 30.82 | 69.43 | — |
| B — Base + RAG | 24.82 | 35.77 | 71.07 | 42.96 |
| C — Fine-tuned, no RAG | 25.91 | 32.98 | 69.72 | — |
| **D — Fine-tuned + RAG** | **32.15** | **38.30** | **71.06** | **42.96** |

### Human Evaluation

| Tiêu chí | Điểm (/ 5) |
|---|:---:|
| Accuracy (Độ chính xác) | 4.2 |
| Completeness (Tính đầy đủ) | 4.1 |
| Fluency (Tính trôi chảy) | 4.3 |
| Helpfulness (Tính hữu ích) | 4.0 |

---

## 📁 Cấu trúc thư mục

```
TDTU-Vietnamese-QA-RAG/
│
├── app.py                        # Gradio demo application
├── requirements.txt              # Python dependencies
├── README.md
│
├── data/
│   ├── text/                     # Tài liệu gốc (20 file .txt)
│   ├── chunks/
│   │   ├── all_chunks.jsonl
│   │   └── parent_chunks.jsonl
│   ├── qa_filtered/
│   │   ├── qa_train.jsonl
│   │   ├── qa_test.jsonl
│   │   └── conversations_train.jsonl
│   └── vector_store/             # FAISS index & metadata
│
├── models/
│   ├── sft_checkpoint/           # SFT adapter (QLoRA)
│   ├── ppo_checkpoint/           # PPO adapter (RLHF)
│   └── reward_model/             # Reward model
│
├── notebooks/
│   ├── 01_setup_data_prep.ipynb
│   ├── 02_qa_generation.ipynb
│   ├── 03_rag_pipeline.ipynb
│   ├── 04_sft_training.ipynb
│   ├── 05_experiments_eval.ipynb
│   ├── 06_rlhf_optional.ipynb
│   └── 07_gradio_demo.ipynb
│
├── results/
│   ├── config_A_results.jsonl
│   ├── eval_summary.json
│   └── human_eval_summary.json
│
├── scripts/
│   ├── chunking.py
│   └── review_ui.py
│
└── visualize/
    ├── visualize.ipynb
    └── figures/
```

---

## 📦 Dependencies

```bash
# Install all dependencies
pip install -r requirements.txt
```

<details>
<summary>Xem danh sách đầy đủ</summary>

```
# Core ML / NLP
transformers==4.45.0
peft==0.13.0
trl==0.11.4
bitsandbytes==0.44.1
accelerate==1.0.1
datasets==3.1.0

# Embeddings & Retrieval
sentence-transformers==3.2.1
faiss-cpu==1.8.0
langchain==0.3.7

# Evaluation
sacrebleu==2.4.3
rouge-score==0.1.2
bert-score==0.3.13

# UI & Utils
gradio==5.5.0
google-genai>=0.3.0
pymupdf>=1.24.0
tqdm>=4.66.0
```

</details>

---

## 📄 Giấy phép

Dự án được phát triển cho mục đích **học thuật**. Tài liệu quy chế TDTU được sử dụng với mục đích nghiên cứu và giáo dục.

---

<div align="center">

**Trần Thiên Hưng · MSSV 52300109**  
Môn Nhập môn Xử lý Ngôn ngữ Tự nhiên (504045) · Đại học Tôn Đức Thắng

</div>
