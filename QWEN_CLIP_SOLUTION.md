# Giải pháp cho lỗi Qwen CLIP

## Lỗi hiện tại:
```
'NoneType' object has no attribute 'load_sd'
```

## Nguyên nhân:
DualCLIPLoader tiêu chuẩn không hỗ trợ Qwen models.

## Giải pháp 1: Dùng FLUX Standard CLIP (KHUYÊN DÙNG)

### Bước 1: Tải CLIP models
Tải 2 files sau:
- `t5xxl_fp16.safetensors`
- `clip_l.safetensors`

Nguồn: Hugging Face FLUX models hoặc CivitAI

### Bước 2: Đặt vào folder
```
ComfyUI/models/clip/t5xxl_fp16.safetensors
ComfyUI/models/clip/clip_l.safetensors
```

### Bước 3: Load workflow
Dùng file: `workflow_flux_standard_clip.json`

---

## Giải pháp 2: Dùng Qwen CLIP (Custom Nodes)

### Option A: ComfyUI-Manager
1. Mở ComfyUI Manager
2. Search "Qwen" hoặc "VL"
3. Cài nodes hỗ trợ Qwen Vision-Language models

### Option B: Manual install
Tìm và cài custom nodes cho Qwen:
- ComfyUI-Qwen
- ComfyUI-VisionLM
- Hoặc các nodes tương tự

---

## Giải pháp 3: Checkpoint đầy đủ

Nếu có file checkpoint đầy đủ (bao gồm UNET + CLIP + VAE):
1. Thay **DualCLIPLoader** + **UNETLoader** + **VAELoader**
2. Bằng **CheckpointLoaderSimple**
3. Load file .safetensors đầy đủ

---

## Khuyến nghị:
✅ **Dùng Giải pháp 1** - FLUX standard CLIP
- Đơn giản nhất
- Tương thích tốt với FLUX UNET
- Không cần custom nodes
