ARG CUDA_VERSION="11.8.0"
ARG CUDNN_VERSION="8"
ARG UBUNTU_VERSION="22.04"
ARG DOCKER_FROM=thebloke/cuda$CUDA_VERSION-ubuntu$UBUNTU_VERSION-pytorch:latest 

# Base pytorch image
FROM $DOCKER_FROM as base

#Workdir
WORKDIR /root/Llama2-Chinese

ARG BRANCH="kh"

# 从git上克隆llama2-chinese仓库
RUN git clone https://github.com/kongh/Llama2-Chinese.git /root/Llama2-Chinese && git checkout $BRANCH

# 使用pip安装requirements.txt
RUN pip install gekko pandas ninja && pip install -r requirements.txt


# Install AutoGPTQ, overwriting the version automatically installed by text-generation-webui
ARG AUTOGPTQ="0.3.0"
ENV CUDA_VERSION=""
ENV GITHUB_ACTIONS=true
ENV TORCH_CUDA_ARCH_LIST="8.0;8.6+PTX;8.9;9.0" 
RUN pip3 uninstall -y auto-gptq && \
    pip3 install --no-cache-dir auto-gptq==$AUTOGPTQ

#克隆Hugging Face仓库
RUN  git clone https://huggingface.co/FlagAlpha/Llama2-Chinese-7b-Chat

#开启7860端口
EXPOSE 7860

#设置启动命令
ENTRYPOINT ["python", "examples/chat_gradio.py", "--model_name_or_path", "/root/Llama2-Chinese/Llama2-Chinese-7b-Chat/"]