# Rawtonic models

Model files for [Rawtonic](https://rawtonic.cc)'s optional local AI tools and
the ONNX Runtime libraries its opt-in build bundles. Core editing in Rawtonic
needs none of them.

Every file is either byte-identical to its authors' release or exported from
their published checkpoint, by `scripts/models/build_model_pack.py` in the
Rawtonic repository (which checks the SHA-256 of every source). The files are
attached to the releases of this repository; `models.json` lists each file
with its size, SHA-256, source and license, and the app verifies the SHA-256
of every download.

| File | Used for | Source | License |
|---|---|---|---|
| `sam2.1_tiny_encoder.onnx`, `sam2.1_tiny_decoder.onnx` | Click and box masks | Exported from Meta's SAM 2.1 Hiera-T checkpoint (`sam2.1_hiera_tiny.pt`, facebookresearch/sam2 at 2b90b9f) by `scripts/models/export_sam2.py`, fp16 storage | Apache-2.0 |
| `birefnet_subject.onnx` | Subject and background masks | BiRefNet general checkpoint (Peng Zheng et al., `ZhengPeng7/BiRefNet`, revision e2bf8e4), exported by `scripts/models/export_birefnet.py`, fp16 storage | MIT |
| `birefnet_fp16_coreml_*` (Core ML package, three files) | Subject and background masks on macOS (builds from 2026-10-11) | BiRefNet general checkpoint (revision e2bf8e4), converted by `scripts/models/convert_birefnet_coreml.py`, fp16 (equal to fp32 against subject references) | MIT |
| `birefnet_coreml_*` (Core ML package, three files) | Subject and background masks on macOS (builds before 2026-10-11) | BiRefNet general checkpoint (revision e2bf8e4), converted by `scripts/models/convert_birefnet_coreml.py`, fp32 | MIT |
| `sam_vit_b_01ec64_encoder.onnx`, `sam_vit_b_01ec64_decoder.onnx` | Click and box masks (builds before 2026-10-09) | Exported from Meta's Segment Anything ViT-B checkpoint (`sam_vit_b_01ec64.pth`), per-channel uint8 weights | Apache-2.0 |
| `u2net.onnx` | Foreground mask | U²-Net (Qin et al.), rembg's ONNX export, unchanged | Apache-2.0 (weights), MIT (export) |
| `depth_anything_v2_vits.onnx` | Depth mask | Depth Anything V2 **Small**, fabio-sim's ONNX export, unchanged | Apache-2.0 |
| `siglip2_vision.onnx`, `siglip2_tags.bin` (and `siglip2_text.onnx`, `siglip2_tokenizer.json` for own tags) | Automatic tags | Exported from Google's SigLIP 2 B/16 (`google/siglip2-base-patch16-224`, revision 75de2d5); `siglip2_tags.bin` holds the text embeddings of Rawtonic's built-in tag list | Apache-2.0 |
| `nlmf_coreml_*` (Core ML package, three files), `nlmf.onnx` | AI Denoise | Nonlocal MatchFilter (Sanchez-Beeckman and Buades), Core ML and ONNX conversions by sheldonxxxx/RapidRAW-Nonlocal-Denoise (revision af6bf36), unchanged | MIT |
| `migan_pipeline_v2.onnx` | Object removal | MI-GAN 512 Places2 (Picsart AI Research), its first author's ONNX pipeline (revision 406830d), unchanged | MIT (weights) |
| `skyseg_upernet_convnext_t.onnx` | Sky mask | UperNet + ConvNeXt-T on ADE20K (Meta's checkpoint via `openmmlab/upernet-convnext-tiny`, revision 876ffc5), exported by `scripts/models/export_sky.py`, fp16 storage | MIT |
| `skyseg_coreml_*` (Core ML package, three files) | Sky mask on macOS | UperNet + ConvNeXt-T (revision 876ffc5), converted by `scripts/models/convert_sky_coreml.py`, fp32 | MIT |
| `face_detection_yunet_2023mar.onnx` | People masks (faces) | YuNet 2023mar (Shiqi Yu et al.), OpenCV Zoo at f12e127, unchanged | MIT |
| `face_mesh_v2.onnx`, `selfie_multiclass_256.onnx` | People masks (landmarks; skin, hair, clothes) | MediaPipe Face Mesh V2 and multiclass selfie segmentation (Google, version 1), converted to ONNX by `scripts/models/export_faces.py` | Apache-2.0 |
| `libonnxruntime-*.so/.dylib`, `onnxruntime-*.dll` | Runtime | Microsoft ONNX Runtime 1.22.0 release archives, unchanged | MIT + third-party notices |
| `libonnxruntime-1.30.0-*.so`, `onnxruntime-1.30.0-*.dll` | Runtime on Windows and Linux (GPU plugins need ≥ 1.24.4) | Microsoft ONNX Runtime 1.30.0 release archives, unchanged | MIT + third-party notices |
| `*onnxruntime_providers_webgpu-0.4.0-*` | GPU for the AI models on Windows (Direct3D 12) and Linux (Vulkan) | Microsoft's WebGPU plugin execution provider 0.4.0 (PyPI onnxruntime-ep-webgpu), library unchanged; the DirectX Shader Compiler DLLs are not included | MIT (includes Dawn, BSD-3-Clause) |
| `*onnxruntime_providers_cuda-0.2.0-cuda13-*` | NVIDIA GPUs, optional (`install_models.sh --cuda`), with the user's own CUDA 13 and cuDNN 9 | Microsoft's CUDA plugin execution provider 0.2.0, unchanged; CUDA/cuDNN not included | MIT |

License texts are in `licenses/`. Training-data terms of the original models
(for example SA-1B for Segment Anything and SAM 2, DIS5K for BiRefNet, Places2 for MI-GAN, ADE20K for the sky model, WIDER FACE for YuNet) are their authors' and are listed in
Rawtonic's `docs/MODELS.md`.
