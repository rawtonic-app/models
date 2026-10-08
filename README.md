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
| `sam_vit_b_01ec64_encoder.onnx`, `sam_vit_b_01ec64_decoder.onnx` | Subject mask | Exported from Meta's Segment Anything ViT-B checkpoint (`sam_vit_b_01ec64.pth`), per-channel uint8 weights | Apache-2.0 |
| `u2net.onnx` | Foreground mask | U²-Net (Qin et al.), rembg's ONNX export, unchanged | Apache-2.0 (weights), MIT (export) |
| `depth_anything_v2_vits.onnx` | Depth mask | Depth Anything V2 **Small**, fabio-sim's ONNX export, unchanged | Apache-2.0 |
| `siglip2_vision.onnx`, `siglip2_tags.bin` (and `siglip2_text.onnx`, `siglip2_tokenizer.json` for own tags) | Automatic tags | Exported from Google's SigLIP 2 B/16 (`google/siglip2-base-patch16-224`, revision 75de2d5); `siglip2_tags.bin` holds the text embeddings of Rawtonic's built-in tag list | Apache-2.0 |
| `libonnxruntime-*.so/.dylib`, `onnxruntime-*.dll` | Runtime | Microsoft ONNX Runtime 1.22.0 release archives, unchanged | MIT + third-party notices |

License texts are in `licenses/`. Training-data terms of the original models
(for example SA-1B for Segment Anything) are their authors' and are listed in
Rawtonic's `docs/MODELS.md`.
