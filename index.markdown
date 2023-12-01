---
layout: default
---

# vLLM AMD Port

We've ported vLLM to AMD hardware and open sourced it here.

## Instructions To Compile the Code from Source

Put instructions here.

# Demo
## MPT-7B-Instruct Fine Tuning Using Jupyter Notebook
[Video Demo](https://www.loom.com/share/29ce1195945d4971ab0675b2a9565ff4?sid=547557a9-bc7c-4eef-b08f-dc8d7667cc30)

In this demo, we fine-tuned the MPT-7B-Instruct model to perform classification tasks by only training the last tranformer layer of the model. We ran the demo in Colossus Cloud with 4 AMD Mi250 GPU cards.

The notebook used in the demo is adopted from [@Vrsen](https://www.youtube.com/watch?v=3de0Utr9XnI) and you can find the original Google Colab notebook [here](https://colab.research.google.com/drive/1DqKNPOzyMUXmJiJFvJITOahVDxCrA-wA).

## Run Inference with Llama 2 70B Using Our vLLM AMD Port
[Video Demo](https://www.loom.com/share/463626f7871e4340b79fe0f6f22129b1?sid=282284e5-abc6-4f31-8199-f863da792692)

In this video, we showed running inference for Llama 2 70B through our [vLLM AMD port](#vllm-amd-port) and connecting [Chatbot UI](https://github.com/mckaywrigley/chatbot-ui) to the model backend. We made some tweaks to make Chatbot UI to work with a custom backend model. To test it out yourselve, please [sign up](#sigup-to-use-colossus) for the free trial version of Colossus.

# Sigup to Use Colossus
You can test our baby Colossus cloud service for free for two days. Baby Colossus will have all environment configured for you to run inference using vLLM and train model using the Jupyter notebook for free. Plese sign up our waitlist [here](https://forms.gle/EVHTfyW1fmXEzwRQ7).

* * *
[Slack](https://join.slack.com/t/colossus-9h48252/shared_invite/zt-27wnzrsuv-Du4mNSac87lqYksM83I2pw)   
[Discord](https://discord.gg/BzAtTH9XW3)