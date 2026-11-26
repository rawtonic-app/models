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

A new pack is a new release; earlier releases stay unchanged for the builds
that point to them. `pack-2026-10-18` holds every file of `pack-2026-10-08`
(same SHA-256) plus the mixed-precision MatchFilter package.
`pack-2026-11-19` holds every file of `pack-2026-11-12` unchanged
plus Rawtonic's own ONNX Runtime 1.30.0 builds without telemetry.
`pack-2026-11-20` holds every file of `pack-2026-11-19` unchanged
plus the fp16 Super Resolution graph for GPU providers.
`pack-2026-11-21` holds every file of `pack-2026-11-20` unchanged
plus the landscape-parts model.
`pack-2026-11-21b` holds every file of `pack-2026-11-21` unchanged
plus the face retouch's eye and teeth parser.
`pack-2026-11-25` holds every file of `pack-2026-11-21b` unchanged
plus Super Resolution's single-pass CPU graph.
`pack-2026-11-25b` holds every file of `pack-2026-11-25` unchanged
plus AI Denoise's Fast network.
`pack-2026-11-25c` holds every file of `pack-2026-11-25b` unchanged
plus the Core ML program of Super Resolution's Fast quality.
`pack-2026-11-25d` holds every file of `pack-2026-11-25c` unchanged
plus AI Denoise's X-Trans network.
The latest, `pack-2026-11-26`, holds every file of `pack-2026-11-25d` unchanged
plus the SAM 2.1 encoder's Core ML program; `models.json` here lists that pack.

| File | Used for | Source | License |
|---|---|---|---|
| `sam2.1_tiny_encoder.onnx`, `sam2.1_tiny_decoder.onnx` | Click and box masks | Exported from Meta's SAM 2.1 Hiera-T checkpoint (`sam2.1_hiera_tiny.pt`, facebookresearch/sam2 at 2b90b9f) by `scripts/models/export_sam2.py`, fp16 storage | Apache-2.0 |
| `sam2_tiny_encoder_coreml_*` (Core ML package, three files) | Click and box masks on macOS (image encoder; from pack-2026-11-26) | The same SAM 2.1 Hiera-T checkpoint converted by `scripts/models/convert_sam2_coreml.py`, fp32 compute. **Changed:** weights rounded to fp16 as in the ONNX encoder, position embedding computed once and kept as a constant, float32 input | Apache-2.0 |
| `birefnet_subject.onnx` | Subject and background masks | BiRefNet general checkpoint (Peng Zheng et al., `ZhengPeng7/BiRefNet`, revision e2bf8e4), exported by `scripts/models/export_birefnet.py`, fp16 storage | MIT |
| `birefnet_fp16_coreml_*` (Core ML package, three files) | Subject and background masks on macOS (builds from 2026-10-11) | BiRefNet general checkpoint (revision e2bf8e4), converted by `scripts/models/convert_birefnet_coreml.py`, fp16 (equal to fp32 against subject references) | MIT |
| `birefnet_coreml_*` (Core ML package, three files) | Subject and background masks on macOS (builds before 2026-10-11) | BiRefNet general checkpoint (revision e2bf8e4), converted by `scripts/models/convert_birefnet_coreml.py`, fp32 | MIT |
| `sam_vit_b_01ec64_encoder.onnx`, `sam_vit_b_01ec64_decoder.onnx` | Click and box masks (builds before 2026-10-09) | Exported from Meta's Segment Anything ViT-B checkpoint (`sam_vit_b_01ec64.pth`), per-channel uint8 weights | Apache-2.0 |
| `u2net.onnx` | Foreground mask | U²-Net (Qin et al.), rembg's ONNX export, unchanged | Apache-2.0 (weights), MIT (export) |
| `relight_geometry_da3_base.onnx` | Relight relative scene geometry and camera | DA3 Base (ByteDance Seed, `depth-anything/DA3-BASE` revision `f4a6c9b3c95e41c82048423d3493a81ec3fa810e`), exported by `scripts/models/export_relight.py`; cached analysis, not metric ground truth | Apache-2.0 |
| `relight_people_normals_david_base.onnx` | Relight surface normals of people | DAViD Base normal model (Microsoft, github.com/microsoft/DAViD at `20a3eb6`), the rights holder's ONNX file, unchanged; trained on synthetic SynthHuman (CDLA-2.0) | MIT |
| `depth_anything_v2_vits.onnx` | Depth mask | Depth Anything V2 **Small**, fabio-sim's ONNX export, unchanged | Apache-2.0 |
| `siglip2_vision.onnx`, `siglip2_tags.bin` (and `siglip2_text.onnx`, `siglip2_tokenizer.json` for own tags) | Automatic tags | Exported from Google's SigLIP 2 B/16 (`google/siglip2-base-patch16-224`, revision 75de2d5); `siglip2_tags.bin` holds the text embeddings of Rawtonic's built-in tag list | Apache-2.0 |
| `nlmf_coreml_*` (Core ML package, three files), `nlmf.onnx` | AI Denoise | Nonlocal MatchFilter (Sanchez-Beeckman and Buades), Core ML and ONNX conversions by sheldonxxxx/RapidRAW-Nonlocal-Denoise (revision af6bf36), unchanged | MIT |
| `nlmf_mixed640_macos13_coreml_*` (Core ML package, three files) | AI Denoise version 3 on macOS 13 and later (from pack-2026-10-18) | The same Nonlocal MatchFilter checkpoint (authors' release v1.0.0), converted by `scripts/models/convert_nlmf_coreml.py`: convolutions fp16, matching fp32, 640 px tiles; the bytes repeat in every conversion. `nlmf_mixed640_coreml_*` is an earlier macOS 15 build (same weights), used by no build | MIT |
| `migan_pipeline_v2.onnx` | Object removal | MI-GAN 512 Places2 (Picsart AI Research), its first author's ONNX pipeline (revision 406830d), unchanged | MIT (weights) |
| `skyseg_upernet_convnext_t.onnx` | Sky mask | UperNet + ConvNeXt-T on ADE20K (Meta's checkpoint via `openmmlab/upernet-convnext-tiny`, revision 876ffc5), exported by `scripts/models/export_sky.py`, fp16 storage | MIT |
| `landscape_upernet_convnext_t.onnx` | Landscape mask (mountains, water, vegetation, natural and artificial ground, architecture, sky; from pack-2026-11-21) | The Sky model's UperNet + ConvNeXt-T ADE20K checkpoint (Meta, facebookresearch/ConvNeXt; transformers conversion `openmmlab/upernet-convnext-tiny`, revision 876ffc5). **Changed head:** exported by `scripts/models/export_landscape.py` with seven part probabilities, each the 150-class softmax summed over the part's ADE20K classes, fp16 storage. The authors do not endorse Rawtonic | MIT (`licenses/ConvNeXt-MIT.txt`) |
| `easyportrait_fpn_1024.onnx`, `easyportrait_coreml_*` (Core ML package, three files) | Face retouch Eyes and Teeth (from pack-2026-11-21b) | EasyPortrait FPN + ResNet-50 face parsing checkpoint `fpn-fp-1024.pth` (SberDevices: A. Kapitanov, K. Kvanchiani, E. Petrova, K. Efremyan, A. Sautin; github.com/hukenovs/easyportrait at 1b3d3e5). **Changed:** re-implemented in plain PyTorch, optimizer state removed, normalisation, mirror pass and eye-class merge in the graph, converted by `scripts/models/export_easyportrait.py` (ONNX, fp16 storage) and `convert_easyportrait_coreml.py` (Core ML, fp16). The authors do not endorse Rawtonic | CC BY-SA 4.0 for these converted files (`licenses/easyportrait-CC-BY-SA-4.0.txt`); original weights under the authors' variant of CC BY-SA 4.0 (`licenses/easyportrait-LICENSE.txt`) |
| `skyseg_coreml_*` (Core ML package, three files) | Sky mask on macOS | UperNet + ConvNeXt-T (revision 876ffc5), converted by `scripts/models/convert_sky_coreml.py`, fp32 | MIT |
| `face_detection_yunet_2023mar.onnx` | People masks (faces) | YuNet 2023mar (Shiqi Yu et al.), OpenCV Zoo at f12e127, unchanged | MIT |
| `face_mesh_v2.onnx`, `selfie_multiclass_256.onnx` | People masks (landmarks; skin, hair, clothes) | MediaPipe Face Mesh V2 and multiclass selfie segmentation (Google, version 1), converted to ONNX by `scripts/models/export_faces.py` | Apache-2.0 |
| `libonnxruntime-*.so/.dylib`, `onnxruntime-*.dll` | Runtime | Microsoft ONNX Runtime 1.22.0 release archives, unchanged | MIT + third-party notices |
| `libonnxruntime-1.30.0-*.so`, `onnxruntime-1.30.0-*.dll` | Runtime on Windows and Linux (GPU plugins need ≥ 1.24.4) | Microsoft ONNX Runtime 1.30.0 release archives, unchanged | MIT + third-party notices |
| `libonnxruntime-1.30.0-notelemetry-*.so/.dylib`, `onnxruntime-1.30.0-notelemetry-*.dll` | Runtime on Apple silicon Macs, Linux x86_64/aarch64 and Windows x64, bundled by Rawtonic's opt-in build (from pack-2026-11-19) | ONNX Runtime 1.30.0 (commit f2c39fe), **built by Rawtonic without telemetry** (`scripts/models/build_onnxruntime.sh`, `build_onnxruntime.ps1` on Windows) | MIT + third-party notices |
| `*onnxruntime_providers_webgpu-0.4.0-*` | GPU for the AI models on Windows (Direct3D 12) and Linux (Vulkan) | Microsoft's WebGPU plugin execution provider 0.4.0 (PyPI onnxruntime-ep-webgpu), library unchanged; the DirectX Shader Compiler DLLs are not included | MIT (includes Dawn, BSD-3-Clause) |
| `superres_compact_x2.onnx`, `superres_coreml_*` (Core ML package, three files) | Super Resolution (export option, 2× enlargement; from pack-2026-11-09) | **2xBHI_small_compact_pretrain by Philip Hofmann** (github.com/Phhofm/models, release `2xBHI_small_compact_pretrain`). **Changed:** exported by `scripts/models/export_superres.py` with each tile's four right-angle rotations enlarged, averaged and clamped (ONNX fp32, Core ML fp16); weights unchanged. The author does not endorse Rawtonic | CC BY 4.0 (`licenses/2xBHI-small-compact-pretrain-CC-BY-4.0.txt`) |
| `superres_compact_x2_fp16.onnx` | Super Resolution on a GPU provider (CUDA, WebGPU; from pack-2026-11-20) | Same Philip Hofmann weights and four-rotation average as above. **Changed conversion:** `scripts/models/convert_superres_fp16.py` converts `superres_compact_x2.onnx` to fp16 weights and fp16 computation with fp32 input and output. The author does not endorse Rawtonic | CC BY 4.0 (`licenses/2xBHI-small-compact-pretrain-CC-BY-4.0.txt`) |
| `superres_compact_x2_cpu.onnx` | Super Resolution on CPU (from pack-2026-11-12) | Same Philip Hofmann weights and four-rotation average as above. **Changed conversion:** `export_superres.py --cpu-only` runs each rotation separately (batch size one), fp32; clamped to 0..1. The author does not endorse Rawtonic | CC BY 4.0 (`licenses/2xBHI-small-compact-pretrain-CC-BY-4.0.txt`) |
| `superres_compact_x2_cpu_single.onnx` | Super Resolution, Fast quality, on ONNX Runtime (from pack-2026-11-25; its manifest entry there still names the first, CPU-only use) | Same Philip Hofmann weights as above. **Changed conversion:** `export_superres.py --single`, each tile enlarged once without the rotation average, clamped, ONNX fp32 | CC BY 4.0 (`licenses/2xBHI-small-compact-pretrain-CC-BY-4.0.txt`) |
| `superres_single_coreml_*` (Core ML package, three files) | Super Resolution, Fast quality, on macOS (from pack-2026-11-25c) | Same Philip Hofmann weights as above. **Changed conversion:** `export_superres.py --single`, each tile enlarged once without the rotation average, clamped, Core ML ML program fp16 | CC BY 4.0 (`licenses/2xBHI-small-compact-pretrain-CC-BY-4.0.txt`) |
| `fast_denoise_v1.onnx` | AI Denoise, Fast quality (recipes with `aiDenoiseQuality: "fast"`; from pack-2026-11-25b) | **Rawtonic's own network** (U-Net, 9.6 M parameters, `scripts/models/fast_denoise_net.py`), trained by Rawtonic by distillation from Nonlocal MatchFilter (Sánchez-Beeckman and Buades, MIT) on 110 noisy Bayer frames of RawNIND (B. Brummer and C. De Vleeschouwer, UCLouvain, doi:10.14428/DVN/DEQCIM, CC BY-SA 4.0). **Changed:** trained weights converted to ONNX (opset 17, fp32). Neither the dataset's nor MatchFilter's authors endorse Rawtonic | CC BY-SA 4.0 (`licenses/fast-denoise-CC-BY-SA-4.0.txt`) |
| `xtrans_denoise_v1.onnx` | AI Denoise on X-Trans sensors (recipes with `aiDenoiseVersion` 6 or later; from pack-2026-11-25d) | **Rawtonic's own network** (U-Net on the X-Trans mosaic, 9.6 M parameters, `scripts/models/xtrans_denoise_net.py`), trained by Rawtonic with supervision on the X-Trans frames of 96 scenes of RawNIND (B. Brummer and C. De Vleeschouwer, UCLouvain, doi:10.14428/DVN/DEQCIM, CC BY-SA 4.0): noisy frames as inputs, the scenes' clean frames as targets. **Changed:** trained weights converted to ONNX (opset 17, fp32, 1008-sample tiles). The dataset's authors do not endorse Rawtonic | CC BY-SA 4.0 (`licenses/xtrans-denoise-CC-BY-SA-4.0.txt`) |
| `*onnxruntime_providers_cuda-0.2.0-cuda13-*` | NVIDIA GPUs, optional (`install_models.sh --cuda`), with the user's own CUDA 13 and cuDNN 9 | Microsoft's CUDA plugin execution provider 0.2.0, unchanged; CUDA/cuDNN not included | MIT |

License texts are in `licenses/`. Training-data terms of the original models
(for example SA-1B for Segment Anything and SAM 2, DIS5K for BiRefNet, Places2 for MI-GAN, ADE20K for the sky model, WIDER FACE for YuNet) are their authors' and are listed in
Rawtonic's `docs/MODELS.md`.
