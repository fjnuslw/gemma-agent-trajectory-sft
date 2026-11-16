# Sources and attribution

This documentation release explains a project built on upstream software; it does not claim to invent those capabilities.

- Official competition SDK, tools, ADK loop and independent verifier: [Gemma 4 Developer Agent](https://www.kaggle.com/competitions/gemma-4-developer-agent).
- [PyTorch FSDP2 / fully_shard](https://docs.pytorch.org/docs/2.10/distributed.fsdp.fully_shard.html), checkpointing and SDPA.
- [Transformers Gemma4](https://github.com/huggingface/transformers/blob/v5.13.1/src/transformers/models/gemma4/modeling_gemma4.py), [PEFT LoRA](https://github.com/huggingface/peft/blob/v0.21.2/src/peft/tuners/lora/config.py), compressed-tensors, Tokenizers, Safetensors and Jinja2.
- [vLLM LoRA](https://docs.vllm.ai/en/v0.19.1/features/lora/); the project binds the competition build rather than assuming all builds with the same version are identical.
- [Scott Weeden's public training kit](https://www.kaggle.com/datasets/scottweeden/gemma-4-developer-agent-tool-lora-kit): reference for query-chunked checkpoint attention, target-only chunked CE and related memory techniques. Its published best score was not tied to the exact adapter/script used as a reference; it is not this project's result.
- [Public Serving and LoRA Verification Kit](https://www.kaggle.com/datasets/dmitriigluzdov/gemma-4-serving-and-lora-verification-kit): selected Native/A sources. Existing metadata records CC BY 4.0 for relevant metadata; repository code, model terms and demonstrations retain their respective notices. The dataset itself is not redistributed here.
- [On-Policy Expert Corrections](https://arxiv.org/abs/2512.14895): research context for student-state corrections; its results are not transferred to this project's settings. This project used offline reconstructed corrections and SFT, not online RL.

The MIT license in this repository applies to original explanatory material and newly authored synthetic teaching code. It does not relicense third-party excerpts, upstream packages, task repositories, model weights or datasets. Source excerpts are bounded and redacted; upstream copyrights and licenses continue to apply. No full third-party package source is bundled.

No proprietary teacher private thinking was collected. Public action explanations are explicitly authored training content and are distinct from private model reasoning.
