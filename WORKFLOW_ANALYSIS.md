# Phân tích Workflow Mẫu (2025-11-15 13_38.json)

## Models & Loaders

### UNET
- **Type**: SeaArtUnetLoader
- **Model**: `Wan2.2 I2V A14B GGUF-high_noise-Q4_K_M`
- **Model 2**: `Wan2.2 I2V A14B GGUF-low_noise-Q4_K_M`

### CLIP
- **Type**: CLIPLoader
- **Model**: `umt5_xxl_fp8_e4m3fn_scaled.safetensors`
- **Type**: `wan`

### VAE
- **Type**: VAELoader
- **Model**: `wan_2.1_vae.safetensors`

### LoRA
1. **Wan21_I2V_14B_lightx2v_cfg_step_distill_lora_rank64-v1**
   - Strength: 1.1

2. **Wan2.1_I2V_14B_FusionX_LoRA-0.1**
   - Strength: 1.0

3. **NSFW/Female ???????? helper for Wan T2V/I2V-v1.0 i2v**
   - Strength: 0.9

## Image Processing

### Gộp ảnh thành Grid
- **Node**: CR Image Grid Panel (từ ComfyUI-ComfyRoll)
- **Parameters**:
  - Columns: 7 (hoặc 6 cho 18 ảnh)
  - Border: black
  - Background: #000000

### Scale ảnh
- **Node**: LayerUtility: ImageScaleByAspectRatio V2
- Dùng để resize ảnh trước khi gộp

## Workflow Đã Tạo

### workflow_18poses_with_grid.json
- ✅ Giữ nguyên 18 prompts và samplers
- ✅ Thêm ImageBatch nodes để gộp 18 ảnh
- ✅ Thêm CR Image Grid Panel (3 rows x 6 columns)
- ✅ Lưu 1 ảnh grid cuối cùng
- ⚠️ Vẫn dùng Qwen models (cần sửa CLIP type như đã làm trước)

## Yêu Cầu

### Custom Nodes Cần Thiết
1. **ComfyUI-ComfyRoll** - Cho CR Image Grid Panel
   - Install: ComfyUI Manager -> Search "ComfyRoll"
   - GitHub: https://github.com/Suzie1/ComfyUI_Comfyroll_CustomNodes

### Models Cần Tải (nếu dùng setup từ workflow mẫu)
1. Wan2.2 I2V A14B GGUF models
2. umt5_xxl_fp8_e4m3fn_scaled.safetensors
3. wan_2.1_vae.safetensors
4. Các LoRA files

## Lưu Ý

- Workflow mẫu làm Image-to-Video, nhưng ta chỉ lấy cách gộp ảnh
- CR Image Grid Panel nhận batch images và tạo grid tự động
- Grid size: 3x6 = 18 ảnh
