# miniGPT4_guide
miniGPT4的本地复现

# 克隆项目
```python
git clone https://github.com/Vision-CAIR/MiniGPT-4.git
```

# 创建环境
```python
cd MiniGPT-4
conda env create -f environment.yml
conda activate minigpt4
```

# 下载权重
#提供一下百度云链接
#建议直接打开网页链接下载下载 git下载有点慢 下载13的成功转换了权重
```python
git lfs install #用于下载大文件 可以不用也能下载
git clone https://huggingface.co/lmsys/vicuna-13b-delta-v0
git clone https://huggingface.co/decapoda-research/llama-13b-hf
```


>> !!!!!! Note
>> 下载下来之后需要把 llama-13b-hf/tokenizer_config.json文件中的"tokenizer_class": "LLaMATokenizer" 改成 "tokenizer_class": "LlamaTokenizer".
>> 不然在转换权重的时候会遇到这个错误
>> ValueError: Tokenizer class LLaMATokenizer does not exist or is not currently imported.


```python
git clone https://huggingface.co/lmsys/vicuna-7b-delta-v0
git clone https://huggingface.co/decapoda-research/llama-7b-hf
```

# 转换权重
```python
pip install git+https://github.com/lm-sys/FastChat.git@v0.1.10 #下载不下来
pip install fastchat
```

## 成功
```python
python -m fastchat.model.apply_delta --base llama-13b-hf/  --target vicuna  --delta vicuna-13b-delta-v0/
```

## 失败
```python
python -m fastchat.model.apply_delta --base llama-13b-hf/  --target vicuna  --delta vicuna-13b-delta-v0/
#其中有一个小错误
https://github.com/Vision-CAIR/MiniGPT-4/issues/96 #没解决
```


# 运行demo
```python
python demo.py --cfg-path eval_configs/minigpt4_eval.yaml  --gpu-id 0
```

会在终端有两个链接
```cmd
Load BLIP2-LLM Checkpoint: pretrained_minigpt4.pth
Initialization Finished
Running on local URL:  http://127.0.0.1:7860
Running on public URL: https://ed51bb7ea87d38e79b.gradio.live
```
使用local的这个网址打开网页 在左侧上传完图片后在右边对话框输入指令
