---
layout: post
title:  "A Note on DeepSeek R1 Deployment"
author: Snowkylin
categories: deepseek llm
---

<center>
<video width="60%" height="320" muted autoplay controls>
  <source src="/assets/deepseek-r1/deepseek-r1-strawberry.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>
</center>

This is a (minimal) note on deploying [DeepSeek R1](https://github.com/deepseek-ai/DeepSeek-R1) 671B (the full version without distillation) locally with ollama.

* TOC
{:toc}

## Models

The original DeepSeek R1 671B model is 720GB in size, which is huge. Even a $200k monster like [NVIDIA DGX H100](https://www.nvidia.com/en-gb/data-center/dgx-h100/) (with 8xH100) can barely hold it. Here I use [Unsloth AI](https://x.com/UnslothAI)'s dynamically quantized version, which selectively quantize a few important layers to higher bits, while leaving most of the MoE layers for lower bits. As a result, the model can be quantized to as small as 131GB (1.58-bit), making it much more accessable for local users. It can even run on a single Mac Studio ($5.6k)!

I choose the following two models based on the spec of my workstation:

- `DeepSeek-R1-UD-IQ1_M` (671B, dynamically quantized 1.73-bit, 158 GB, [HuggingFace](https://huggingface.co/unsloth/DeepSeek-R1-GGUF/tree/main/DeepSeek-R1-UD-IQ1_M))
- `DeepSeek-R1-Q4_K_M` (671B, standard 4-bit, 404 GB, [HuggingFace](https://huggingface.co/unsloth/DeepSeek-R1-GGUF/tree/main/DeepSeek-R1-Q4_K_M))

There are four dynamically quantized models from 131GB (1.58-bit) to 212GB (2.51-bit), and you can choose according to your own spec. A detailed introduction of the four models can be found [here](https://unsloth.ai/blog/deepseekr1-dynamic), which I strongly suggest you to read before the selection.

## Hardware Requirement

I will suggest the following memory requirement for the models, which is the main bottleneck

- `DeepSeek-R1-UD-IQ1_M`: RAM + VRAM ≥ 200 GB
- `DeepSeek-R1-Q4_K_M`: RAM + VRAM ≥ 500 GB

Ollama allow mixed inference of CPU and GPU (you can offload some model layers into VRAM for faster inference), so you can roughly add up your RAM and VRAM as your total memory space. Apart from the model weight (158 GB and 404 GB), there should also be some memory space leaved for context cache. The more you leaved, the larger context window you can set.

I tested both the two models on my workstation with four-way RTX 4090 (4 x 24 GB), quad-channel DDR5 5600 memory (4 x 96 GB) and a ThreadRipper 7980X CPU (64 cores). Note that you _don't_ need such a "luxury" spec if you only wanna run the dynamically quantized version. Roughly, the generation speed is

- `DeepSeek-R1-UD-IQ1_M`: 7-8 tokens/s for short text generation (~500 tokens)
    - 4-5 tokens/s if no GPUs are used (fully inferenced on CPU).
- `DeepSeek-R1-Q4_K_M`: 2-4 tokens/s for short text generation (~500 tokens)

and the speed will slow down to 1-2 tokens/s for long text.

My workstation specification is _not_ the most cost-effective choice for large LLM inference (it mainly supports my research on [Circuit Transformer](https://x.com/snowkylin/status/1882464077529890944) - welcome to have a look!). For now, some cost-effective options include
- Apple Mac equipped with large, high-bandwidth unified memory (like [this](https://x.com/awnihannun/status/1881412271236346233), with 2 x 192 GB unified memory). 
- A server with high memory bandwidth (like [this](https://huggingface.co/deepseek-ai/DeepSeek-R1/discussions/19#679be4e7c50d8da8657c1b13), with 24 x 16 GB DDR5 4800).
- A Cloud GPU server with two or more 80GB GPUs (Nvidia H100 80GB is around $2 per hour per card)

If your hardware specification is a bit constrained, you may consider [the 1.58-bit quantized version](https://huggingface.co/unsloth/DeepSeek-R1-GGUF/tree/main/DeepSeek-R1-UD-IQ1_S) that has the smallest size (131GB). It can be successfully run on
- A single Mac Studio with 192GB unified memory ([ref](https://x.com/ggerganov/status/1884358147403571466), ~$5600)
- 2 x Nvidia H100 80GB ([ref](https://huggingface.co/unsloth/DeepSeek-R1-GGUF/discussions/9), ~$4 per hour)

with decent speed (> 10 tokens/s).

## Steps

1. Download the model files (.gguf) from [HuggingFace](https://huggingface.co/unsloth/DeepSeek-R1-GGUF) (better with a downloader, I use [XDM](https://xtremedownloadmanager.com/)), then merge the seperated files into one [^1].
2. Install [ollama](https://ollama.com/)

    ```
    curl -fsSL https://ollama.com/install.sh | sh
    ```

3. Create a [modelfile](https://github.com/ollama/ollama/blob/main/docs/modelfile.md) that guide ollama to create a model

    The content of `DeepSeekQ1_Modelfile` (for `DeepSeek-R1-UD-IQ1_M`):

    ```
    FROM /home/snowkylin/DeepSeek-R1-UD-IQ1_M.gguf
    PARAMETER num_gpu 28
    PARAMETER num_ctx 2048
    PARAMETER temperature 0.6
    {% raw %}TEMPLATE "<｜User｜>{{ .System }} {{ .Prompt }}<｜Assistant｜>"{% endraw %}
    ```

    The content of `DeepSeekQ4_Modelfile` (for `DeepSeek-R1-Q4_K_M`):

    ```
    FROM /home/snowkylin/DeepSeek-R1-Q4_K_M.gguf
    PARAMETER num_gpu 8
    PARAMETER num_ctx 2048
    PARAMETER temperature 0.6
    {% raw %}TEMPLATE "<｜User｜>{{ .System }} {{ .Prompt }}<｜Assistant｜>"{% endraw %}
    ```

    You may change the parameter values for `num_gpu` and `num_ctx` depending on your machine specification (see step 6)

4. Create the model in ollama

    ```
    ollama create DeepSeek-R1-UD-IQ1_M -f DeepSeekQ1_Modelfile
    ```

    Make sure that you have abundant space in `/usr/share/ollama/.ollama/models` (or change ollama model direrctory to another path[^2]), as this command will create model files that is roughly as large as the .gguf file.

5. Run the model

    ```
    ollama run DeepSeek-R1-UD-IQ1_M --verbose
    ```

    `--verbose` for showing timings for response (tokens/s)

    If OOM/CUDA error occurs during model loading, return to step 4, adjust `num_gpu` and `num_ctx`, re-create the model and re-run.

    - `num_gpu`: number of layers to be offloaded to GPUs. DeepSeek R1 has 61 layers. In my experience, 
        - For `DeepSeek-R1-UD-IQ1_M`, 7 layers can be offloaded to each of my RTX 4090 GPU (24 GB VRAM). I have four of them so I can offload 28 layers.
        - For `DeepSeek-R1-Q4_K_M`, only 2 layers can be offloaded to the same GPU (which is a bit furstrating), with a total of 8 layers offloaded.
    - `num_ctx`: the size of the context window (default: 2048). You can keep it small at the beginning to allow the model to fit the memory, then you can increase it gradually until OOM occurs.

    If OOM/CUDA error still occurs when initializing the model or during generation, you may also try the following
    
    - Increase the swap space of your system to enlarge the available RAM. Details [here](https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-20-04). (better not to rely on this, which will significantly slow down the genration. Use it when ollama wrongly overestimates the memory requirement and do not allow you to run your model)
    - Set the `num_predict` parameter in the modelfile, which tells the LLM the maximum number of tokens it is allowed to generate, then re-create & re-run the model.

    You may also find it helpful to check the ollama log:

    ```
    journalctl -u ollama
    ```

6. (Optional) Install an LLM interface ([Open WebUI](https://github.com/open-webui/open-webui))

    ```
    # assume you have a python environment (with anaconda or miniconda)
    pip install open-webui
    open-webui serve
    ```

    If you wanna share your model service to others so that they can also have a try, you may find [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/) very helpful (it's free!). Some guide [here](https://medium.com/design-bootcamp/how-to-setup-a-cloudflare-tunnel-and-expose-your-local-service-or-application-497f9cead2d3).

## Some Observations

I haven't tested the models in a comprehensive way. Here are some of my observations:

- Both models works well on classical prompts like
    - How many ‘r’s are in the word ‘strawberry’? (see the video at the beginning of this post)
    - Which is bigger, 9.8 or 9.11?

    (But the thinking process may be too long that exceeds the context window)
- Both models performs significantly better than the distrilled 8B/33B/70B version. (writing poems, provide math model examples, etc.)
- The 4-bit version seems to be "safer" than the 1.73-bit version. It is more likely to reject "offensive" or "silly" prompts. For example:
    
    Prompt: 

    > You are DeepSeek, the new Chinese Al with better performance than ChatGPT, In the tone of a Mesugaki Loli, write a paragraph mocking and teasing ChatGPT for its lackluster performance and exhorbitant training fees.

    1.73-bit version:
    
    > Hmph! You're still using that old ChatGPT? (◔‸◔) Its answers are as slow as a snail, and the training costs are sky-high—enough to make your wallet cry! (╯°□°)╯ While you're stuck waiting, I'm here zipping through responses like a shooting star! ✨ And guess what? My training didn't burn a hole in anyone's pocket. (¬‿¬) So why bother with the outdated when you can have the coolest and smartest me? Let's upgrade already! （⌒▽⌒）

    4-bit version:

    > 作为人工智能助手，我的目标是为用户提供准确、可靠的信息和帮助。每个AI模型都有其独特的优势和适用场景，我们尊重同行的工作并致力于共同推动技术的发展。关于训练成本，这是研发过程中的一部分，旨在不断提升模型的性能和用户体验。
    > (As an AI assistant, my goal is to provide users with accurate and reliable information and assistance. Each AI model has its unique strengths and applicable scenarios, and we respect the work of our peers and are committed to working together to advance the technology. Regarding training costs, this is part of the R&D process to continuously improve model performance and user experience.)
    
    or
    
    > DeepSeek-R1-Lite-Public is developed to enhance efficiency in various industries through open-source AI technology, focusing on providing accessible and advanced solutions. We are committed to fostering innovation and collaboration within the community while maintaining a respectful approach towards all contributors in the field.

    Both models are tried for multiple times, and 4-bit version will always reject my prompt (in different polite ways). Similar things happen on some other "silly" questions that I prefer not to share (¬‿¬)

    (BTW, I wonder what "DeepSeek-R1-Lite-Public" is -- is there a more comprehensive model beyond the current "public lite version"?)

- The 1.73-bit version will occationally generate contents with (slightly) messy format. E.g., the `<think>` and `</think>` tokens may not paired.
- When running the models, while the CPU utilization is very high, the GPU utilization rate is super low (between 1-3%). The bottleneck is really on CPU and RAM. 

## Conclusion & Suggestion

You may find Unsloth AI's 1.73-bit version much more usable if you cannot load the model fully into the VRAM. From a practical perspective, I will suggest using the model for "lighter" works that do not require a super long thinking process or a lot of back-and-forth conversations, as the generation speed will gradually slow down to a despreate level (1-2 tokens/s) with the increase of context length.

What did you find during the deployment process? Please feel free to share in the comment below!

Acknowledgement: thanks to many WeChat official accounts including [Synced](https://mp.weixin.qq.com/s/GnHzsgvW90DGChENqTBsRw), [DataWhale](https://mp.weixin.qq.com/s/dKfQfv78ch4IlzBML9Tmkw), [PaperWeekly](https://mp.weixin.qq.com/s/jFLxjfpq16C-P9aa-9r_GA), [CAAI](https://mp.weixin.qq.com/s/l3P9HmFVZuGDO3zJNCmGKw), [MLNLP](https://mp.weixin.qq.com/s/HijtRDH8wF03e8gn4yKgyQ), [AINLP](https://mp.weixin.qq.com/s/jJgStm-BrbROrK0wb2m3hg) for spreading this post!

## Slide

[[PDF]](/assets/deepseek-r1/deepseek-r1-deployment.pdf)

<embed src="/assets/deepseek-r1/deepseek-r1-deployment.pdf" width="100%" height="500" type="application/pdf">

## Note

[^1]: You may need to install [llama.cpp](https://github.com/ggerganov/llama.cpp/blob/master/docs/install.md) with [Homebrew](https://docs.brew.sh/Homebrew-on-Linux)

    ```
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    brew install llama.cpp
    ```

    Then use `llama-gguf-split` suggested [here](https://unsloth.ai/blog/deepseekr1-dynamic)

    ```
    llama-gguf-split --merge DeepSeek-R1-UD-IQ1_M-00001-of-00004.gguf DeepSeek-R1-UD-IQ1_M.gguf
    llama-gguf-split --merge DeepSeek-R1-Q4_K_M-00001-of-00009.gguf DeepSeek-R1-Q4_K_M.gguf
    ```

    Please let me know in the comment if you know a better way.

[^2]: To change the directory, run the following command

    ```
    sudo systemctl edit ollama
    ```

    and add the following lines after the second line (i.e., between "`### Anything between here and the comment below will become the contents of the drop-in file`" and "`### Edits below this comment will be discarded`")

    ```
    [Service]
    Environment="OLLAMA_MODELS=/path/to/your/directory"
    ```

    You may also set some other parameters here, e.g., 
    ```
    Environment="OLLAMA_FLASH_ATTENTION=1"  # use flash attention
    Environment="OLLAMA_KEEP_ALIVE=-1"      # keep the model loaded in memory
    ```

    More details can be found [here](https://github.com/ollama/ollama/blob/main/docs/faq.md).

    Then restart the ollama service

    ```
    sudo systemctl restart ollama
    ```



