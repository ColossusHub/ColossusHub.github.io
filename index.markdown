---
layout: default
---
# Our Mission
In response to the high demand for GPU cards for Large Language Model (LLM) training and referencing, and the scarcity of NVIDIA GPUs, we are a team of engineers dedicated to bridging the software gap for AMD hardware. Although AMD hardware is more readily available, it often lacks compatibility with most popular LLM frameworks.

Our initial step towards this goal involved porting the widely-used LLM framework, [vLLM](https://github.com/vllm-project/vllm) (which supports PagedAttention, FlashAttention2, and continuous batching), to AMD hardware. We established a server named Colossus, equipped with 4 Mi250 GPUs. We evaluated its performance using a benchmarking script from LLM against NVIDIA's A100(40G) and A100(80G), focusing on the Llama2 13B and Llama2 70B models. Below are the performance graphs we obtained:

![Llama 2 13B Throughput](./assets/llama2-13b-throughput.png){:width="490x" height="348px"}
![Llama 2 70B Throughput](./assets/llama2-70b-throughput.png){:width="490px"}

The graph illustrates that for the 13B model, we compare single GPU cards. In the case of the 70B model, the comparison is between 8 NVIDIA cards and 4 AMD cards. This is primarily because the total RAM of the 4 AMD cards falls between that of 8 NVIDIA 40G and 8 NVIDIA 80G cards. Notably, as the model size increases, the performance gap diminishes, likely due to AMD GPUs having fewer nodes and higher bandwidth per node, leading to reduced overall communication overhead. Below are the hardware specifications for reference:

![hardware spec](./assets/hardware-specs.png){:width="990px"}

Through the experiments conducted, we aim to demonstrate that AMD is a cost-effective option, particularly as it is available at a significantly lower price compared to its NVIDIA counterpart.

# Demo
## Fine-Tuning MPT-7B-Instruct with Jupyter Notebook
[Video Demo](https://www.loom.com/share/29ce1195945d4971ab0675b2a9565ff4?sid=547557a9-bc7c-4eef-b08f-dc8d7667cc30)

In this demonstration, we fine-tuned the MPT-7B-Instruct model for classification tasks, focusing exclusively on training the final transformer layer. The demonstration was conducted on Colossus Cloud, utilizing 4 AMD Mi250 GPU cards.

The notebook featured in this demonstration is a modification of one created by [@Vrsen](https://www.youtube.com/watch?v=3de0Utr9XnI). The original Google Colab notebook is accessible [here](https://colab.research.google.com/drive/1DqKNPOzyMUXmJiJFvJITOahVDxCrA-wA).

## Executing Inference with Llama 2 70B on Our vLLM AMD Port
[Video Demo](https://www.loom.com/share/463626f7871e4340b79fe0f6f22129b1?sid=282284e5-abc6-4f31-8199-f863da792692)

This video showcases the execution of inference for Llama 2 70B via our [vLLM AMD port](#vllm-amd-port), integrating the [Chatbot UI](https://github.com/mckaywrigley/chatbot-ui) with the model's backend. We tweaked Chatbot UI a little bit to work with our custom backend model. To experience this yourself, register [here](#register-for-colossus-access) for a free trial of Colossus.

# vLLM AMD Port
We created a branch from the original vLLM repository on October 8, 2003, and continue to support all models that were compatible up to that date. Additionally, we integrated the [ROCm port of AWQ](https://github.com/ColossusHub/vllm/commit/6e6121f942617d0963482f10f20f7184047c29b1), enabling the use of quantized versions of the models with reduced memory requirements. 

## Steps to build the Code from Source

> Please note: our vLLM port has been tested exclusively on AMD Mi210 and Mi250 GPUs.

1. Begin by installing [ROCm 5.7](https://rocm.docs.amd.com/en/latest/deploy/linux/installer/install.html).
2. (Optional) For best practice, consider installing [conda](https://docs.conda.io/projects/miniconda/en/latest/) and executing the following steps in a separate virtual environment.
3. To install [PyTorch with ROCm5.7](https://pytorch.org/get-started/locally/) support, execute the following command:

    ```shell
    pip3 install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/rocm5.7
    ```

4. For installing the ROCm version of [Flash Attention 2](https://github.com/ROCmSoftwarePlatform/flash-attention), follow these steps:

    * Clone the repository:

        ```shell
        git clone https://github.com/ROCmSoftwarePlatform/flash-attention
        ``` 

    * Inside the `flash-attention` repository, execute:

        ```shell
        pip install ninja
        git submodule update --init 
        python3 setup.py install 
        ```

        In case of GPU architecture-related errors, consider modifying the [allowed_archs](https://github.com/ROCmSoftwarePlatform/flash-attention/blob/3d2b6f5d037782cc2c906909a46fb7e2e1b48b25/setup.py#L215) to include only `gfx90a`, which is the architecture for Mi210 and Mi250 GPUs.

    
5. After installing Flash Attention 2 in your conda environment, proceed with the following:

    Clone our [ROCm vLLM repository](https://github.com/ColossusHub/vllm):

    ```shell
    git clone https://github.com/ColossusHub/vllm.git
    ```

    To focus the build specifically for Mi210 or Mi250 cards and reduce build time, you may use `export PYTORCH_ROCM_ARCH=gfx90a`` as an optional step. To install vLLM, navigate (`cd``) into the repository and run:
    
    ```shell
    python setup.py install 
    ```

## Quantization
We have also ported AWQ to ROCM.  Our implementation is built on the ROCM WMMA library to replace the PTX assembly from the original implementation.  We have implemented a matrix multiply with dequantization that supports multiple different macro tile sizes along with prefetching.

# Register for Colossus Access
Experience our Colossus cloud service at no cost for two days. With Baby Colossus, you'll have a fully configured environment ready for running vLLM inferences and training models using Jupyter notebook, all free of charge. Please join our waitlist by signing up [here](https://forms.gle/EVHTfyW1fmXEzwRQ7).

* * *
[Slack](https://join.slack.com/t/colossus-9h48252/shared_invite/zt-27wnzrsuv-Du4mNSac87lqYksM83I2pw)   
[Discord](https://discord.gg/BzAtTH9XW3)