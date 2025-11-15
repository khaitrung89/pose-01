# Workflow 18 Tư Thế Gộp Thành 1 Ảnh

## Tóm Tắt

Tôi đã tạo 2 workflows để sinh 18 tư thế khác nhau và tự động gộp chúng thành 1 ảnh grid (3 hàng x 6 cột):

### 1. workflow_18poses_flux_with_grid.json ⭐ KHUYÊN DÙNG
- Sử dụng FLUX standard CLIP models
- Đã sửa lỗi CLIP type mismatch
- Cần models: `t5xxl_fp16.safetensors` + `clip_l.safetensors`

### 2. workflow_18poses_with_grid.json
- Giữ nguyên Qwen CLIP setup gốc
- Cần custom nodes cho Qwen (nếu có)

## Cách Hoạt Động

```
Input: 18 Prompts khác nhau
    ↓
18 KSamplers → 18 VAEDecoders
    ↓
17 ImageBatch nodes (gộp thành 1 batch)
    ↓
CR Image Grid Panel (tạo grid 3x6)
    ↓
SaveImage → Lưu 1 ảnh duy nhất
```

## Hướng Dẫn Sử Dụng

### Bước 1: Cài Custom Nodes

**ComfyUI-ComfyRoll** (bắt buộc):
```bash
cd ComfyUI/custom_nodes
git clone https://github.com/Suzie1/ComfyUI_Comfyroll_CustomNodes
cd ComfyUI_Comfyroll_CustomNodes
pip install -r requirements.txt
```

Hoặc qua ComfyUI Manager:
1. Mở ComfyUI Manager
2. Search "ComfyRoll"
3. Install "ComfyUI_Comfyroll_CustomNodes"

### Bước 2: Tải Models (nếu dùng FLUX workflow)

Tải 2 CLIP models cho FLUX:
- `t5xxl_fp16.safetensors`
- `clip_l.safetensors`

Đặt vào: `ComfyUI/models/clip/`

### Bước 3: Load Workflow

1. Mở ComfyUI
2. Load workflow: **workflow_18poses_flux_with_grid.json**
3. Chỉnh sửa 18 prompts theo ý muốn
4. Click "Queue Prompt"

### Bước 4: Kết Quả

Ảnh grid sẽ được lưu với tên: `18_poses_grid_flux_*.png`

## So Sánh với Workflow Mẫu

### Workflow Mẫu (2025-11-15 13_38.json)
- **Mục đích**: Image-to-Video
- **Models**: Wan2.2 I2V (SeaArt)
- **LoRA**: 3 LoRAs cho I2V
- **Grid**: CR Image Grid Panel gộp video frames

### Workflow Mới
- **Mục đích**: Text-to-Image với 18 tư thế
- **Models**: FLUX UNET + FLUX CLIP
- **Grid**: CR Image Grid Panel gộp 18 ảnh

## Checkpoint & LoRA Từ Workflow Mẫu

Nếu muốn dùng setup từ workflow mẫu:

### Models
- **UNET**: Wan2.2 I2V A14B GGUF-high_noise-Q4_K_M
- **CLIP**: umt5_xxl_fp8_e4m3fn_scaled.safetensors (type: wan)
- **VAE**: wan_2.1_vae.safetensors

### LoRAs
1. Wan21_I2V_14B_lightx2v_cfg_step_distill_lora_rank64-v1 (1.1)
2. Wan2.1_I2V_14B_FusionX_LoRA-0.1 (1.0)
3. NSFW/Female helper for Wan T2V/I2V-v1.0 i2v (0.9)

**Lưu ý**: Những models này cho Image-to-Video, không phù hợp cho Text-to-Image workflow.

## Tùy Chỉnh Grid

Trong node **CR Image Grid Panel**, bạn có thể thay đổi:

- **Widget 5 (columns)**: Số cột
  - Hiện tại: 6 (tạo grid 3x6 cho 18 ảnh)
  - Thay đổi thành:
    - `3` → grid 6x3
    - `9` → grid 2x9
    - `18` → grid 1x18 (hàng ngang)

- **Widget 2 & 4 (border color)**: Màu viền
  - `"black"`, `"white"`, `"red"`, etc.

- **Widget 6 (background)**: Màu nền
  - `"#000000"` (đen), `"#FFFFFF"` (trắng), etc.

## Troubleshooting

### Lỗi: "CR Image Grid Panel" not found
→ Chưa cài ComfyUI-ComfyRoll
→ Làm theo Bước 1

### Lỗi: CLIP dimension mismatch
→ Dùng **workflow_18poses_flux_with_grid.json**
→ Hoặc xem QWEN_CLIP_SOLUTION.md

### Lỗi: Out of memory
→ Giảm batch size bằng cách:
1. Chạy từng sampler một
2. Hoặc giảm resolution
3. Hoặc dùng UNET quantized (Q4/Q8)

## Links Tải

### Workflows
- [workflow_18poses_flux_with_grid.json](https://github.com/khaitrung89/pose-01/raw/claude/review-file-01LE5JX5ZYd6YZUoTNQV1xjt/workflow_18poses_flux_with_grid.json)
- [workflow_18poses_with_grid.json](https://github.com/khaitrung89/pose-01/raw/claude/review-file-01LE5JX5ZYd6YZUoTNQV1xjt/workflow_18poses_with_grid.json)

### Documents
- [WORKFLOW_ANALYSIS.md](https://github.com/khaitrung89/pose-01/raw/claude/review-file-01LE5JX5ZYd6YZUoTNQV1xjt/WORKFLOW_ANALYSIS.md)
- [QWEN_CLIP_SOLUTION.md](https://github.com/khaitrung89/pose-01/raw/claude/review-file-01LE5JX5ZYd6YZUoTNQV1xjt/QWEN_CLIP_SOLUTION.md)

---

**Tạo bởi**: Claude Code
**Ngày**: 2025-11-15
