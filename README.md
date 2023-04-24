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

# 下载vicuna权重
#建议直接打开网页链接下载下载 git下载有点慢 下载13的成功转换了权重
```python
git lfs install #用于下载大文件 可以不用也能下载
git clone https://huggingface.co/lmsys/vicuna-13b-delta-v0
git clone https://huggingface.co/decapoda-research/llama-13b-hf
```

```
>> !!!!!! Note
>> 下载下来之后需要把 llama-13b-hf/tokenizer_config.json文件中的"tokenizer_class": "LLaMATokenizer" 改成 "tokenizer_class": "LlamaTokenizer".
>> 不然在转换权重的时候会遇到这个错误
>> ValueError: Tokenizer class LLaMATokenizer does not exist or is not currently imported.
```

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

然后，在模型配置文件中设置权重路径为vicuna
[这里](minigpt4/configs/models/minigpt4.yaml#L16) 16行 
 
# 准备预训练MiniGPT-4权重
 
要使用我们的预训练模型，请下载预训练权重
|  Checkpoint Aligned with Vicuna 13B   | Checkpoint Aligned with Vicuna 7B  |
|  ----  | ----  |
| [下载](https://drive.google.com/file/d/1a4zLvaiDBr-36pasffmgpvH5P7CKmpze/view)  | [下载](https://drive.google.com/file/d/1RY9jV0dyqLX-o38LrumkKRh6Jtaop58R/view) |
然后，在评估配置文件中设置预训练权重的路径 
在[eval_configs/minigpt4_eval.yaml](eval_configs/minigpt4_eval.yaml#L10)第11行。

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

# 最后的效果
<img width="1469" alt="image" src="https://user-images.githubusercontent.com/54712081/233894269-57934d43-d593-41b4-9678-2b5bb9c3d0f9.png">

