# FIX LỖI: CLIP Type Mismatch

## Vấn Đề Gốc

Workflow cũ bị lỗi:
```
RuntimeError: Given normalized_shape=[3584], expected input with shape [*, 3584],
but got input of size [2, 256, 4096]
```

**Nguyên nhân:** Mix lẫn Qwen UNET (4096 dim) với FLUX CLIP (3584 dim) → KHÔNG TƯƠNG THÍCH!

## Giải Pháp ✅

Tạo 2 workflows mới với models **TƯƠNG THÍCH**:

### Solution 1: Checkpoint Version ⭐ KHUYÊN DÙNG

**File**: `workflow_18poses_CHECKPOINT.json`

**Ưu điểm:**
- ✅ Dễ dùng nhất
- ✅ Chỉ cần 1 file checkpoint
- ✅ Work với BẤT KỲ checkpoint: FLUX, SDXL, SD1.5, Pony, etc.
- ✅ Không cần lo CLIP type mismatch
- ✅ Không cần tải riêng CLIP, UNET, VAE

**Cách dùng:**
1. Load workflow
2. Click vào **CheckpointLoaderSimple** node
3. Chọn checkpoint bạn có (ví dụ: `flux1-dev-fp8.safetensors`)
4. Chạy workflow

**Checkpoint suggestions:**
- FLUX: `flux1-dev-fp8.safetensors`, `flux1-schnell.safetensors`
- SDXL: `sd_xl_base_1.0.safetensors`, `juggernautXL_v9.safetensors`
- SD1.5: `v1-5-pruned-emaonly.safetensors`
- Pony: `ponyDiffusionV6XL.safetensors`

### Solution 2: Pure FLUX Version

**File**: `workflow_18poses_PURE_FLUX.json`

**Yêu cầu:**
- UNET: `flux1-dev.safetensors` (đặt trong `ComfyUI/models/unet/`)
- CLIP:
  - `t5xxl_fp16.safetensors` (đặt trong `ComfyUI/models/clip/`)
  - `clip_l.safetensors` (đặt trong `ComfyUI/models/clip/`)
- VAE: Tự động từ FLUX

**Cách dùng:**
1. Tải 3 models trên
2. Đặt vào đúng thư mục
3. Load workflow
4. Chạy

## So Sánh Workflows

| Workflow | Độ Khó | Models Cần | Tương Thích | Khuyến Nghị |
|----------|---------|------------|-------------|-------------|
| **workflow_18poses_CHECKPOINT.json** | ⭐ Dễ | 1 checkpoint | Tất cả checkpoints | ✅ Dùng cái này |
| workflow_18poses_PURE_FLUX.json | ⭐⭐ Trung bình | 3 files | Chỉ FLUX | Nếu có FLUX models |
| ~~workflow_18poses_flux_with_grid.json~~ | ❌ BỊ LỖI | Mix lẫn | KHÔNG | ❌ Đừng dùng |

## Tải Models

### FLUX Models
- Hugging Face: https://huggingface.co/black-forest-labs/FLUX.1-dev
- CLIP T5: https://huggingface.co/comfyanonymous/flux_text_encoders

### SDXL Checkpoints
- Stable Diffusion XL: https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0
- CivitAI: https://civitai.com/ (search "SDXL")

### Where to Put Models

```
ComfyUI/
├── models/
│   ├── checkpoints/          # Checkpoint files (.safetensors)
│   │   ├── flux1-dev-fp8.safetensors
│   │   ├── sd_xl_base_1.0.safetensors
│   │   └── ...
│   ├── unet/                 # UNET models (for split loading)
│   │   └── flux1-dev.safetensors
│   ├── clip/                 # CLIP models
│   │   ├── t5xxl_fp16.safetensors
│   │   └── clip_l.safetensors
│   └── vae/                  # VAE models
```

## Output

Cả 2 workflows đều:
- Sinh 18 ảnh với 18 prompts khác nhau
- Gộp thành grid 3x6
- Lưu 1 ảnh cuối cùng: `18_poses_grid_*.png`

## Yêu Cầu Custom Nodes

**ComfyUI-ComfyRoll** (bắt buộc):
```bash
cd ComfyUI/custom_nodes
git clone https://github.com/Suzie1/ComfyUI_Comfyroll_CustomNodes
pip install -r ComfyUI_Comfyroll_CustomNodes/requirements.txt
```

## Troubleshooting

### Lỗi: "CR Image Grid Panel not found"
→ Chưa cài ComfyUI-ComfyRoll
→ Cài theo hướng dẫn trên

### Lỗi: "Checkpoint not found"
→ Tải checkpoint về
→ Đặt vào `ComfyUI/models/checkpoints/`
→ Refresh ComfyUI

### Out of Memory
→ Dùng checkpoint FP8 (nhẹ hơn):
- `flux1-dev-fp8.safetensors` thay vì `flux1-dev.safetensors`
→ Hoặc giảm resolution trong EmptyLatentImage nodes

---

**Updated**: 2025-11-15
**Status**: ✅ WORKING
