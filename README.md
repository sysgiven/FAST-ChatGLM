# FAST-ChatGLM FROM ChatGLM

## 介绍

ChatGLM是一个开源的、支持中英双语的对话语言模型，基于 [General Language Model (GLM)](https://github.com/THUDM/GLM) 架构，具有 62 亿参数。

基于开源模型ChatGLM开发的加速版FAST-ChatGLM已发布，结合模型剪枝、模型量化、模型蒸馏等模型压缩技术，使该模型最低可在无显卡的CPU平台运行，在Nvidia显卡上会得到20倍以上的加速。

## 更新信息
**[2023/12/25]** 发布 [FAST-ChatGLM](https://github.com/sysgiven/FAST-ChatGLM) ，基于 ChatGLM的模型，训练和推理速度全面提升。

### 打包下载

[BaiDu Cloud]https://pan.baidu.com/s/1e3gJOi4qRPL7IKZkO1sK2g?pwd=pxx6 提取码：pxx6

[KuaKe Cloud]https://pan.quark.cn/s/ed85c163b3f6 提取码：Njzt

### 环境安装

使用 pip 安装依赖：`pip install -r requirements.txt`，其中 `transformers` 库版本推荐为 `4.27.1`，但理论上不低于 `4.23.1` 即可。

此外，如果需要在 cpu 上运行量化后的模型，还需要安装 `gcc` 与 `openmp`。多数 Linux 发行版默认已安装。对于 Windows ，可在安装 [TDM-GCC](https://jmeubank.github.io/tdm-gcc/) 时勾选 `openmp`。 Windows 测试环境 `gcc` 版本为 `TDM-GCC 10.3.0`， Linux 为 `gcc 11.3.0`。在 MacOS 上请参考 [Q1](FAQ.md#q1)。

### 代码调用 

可以通过如下代码调用 ChatGLM模型来生成对话：

```python
>>> from transformers import AutoTokenizer, AutoModel
>>> tokenizer = AutoTokenizer.from_pretrained("THUDM/chatglm-6b", trust_remote_code=True)
>>> model = AutoModel.from_pretrained("THUDM/chatglm-6b", trust_remote_code=True).half().cuda()
>>> model = model.eval()
>>> response, history = model.chat(tokenizer, "你好", history=[])
>>> print(response)
你好👋!我是人工智能助手 ChatGLM,很高兴见到你,欢迎问我任何问题。
>>> response, history = model.chat(tokenizer, "晚上睡不着应该怎么办", history=history)
>>> print(response)
晚上睡不着可能会让你感到焦虑或不舒服,但以下是一些可以帮助你入睡的方法:

1. 制定规律的睡眠时间表:保持规律的睡眠时间表可以帮助你建立健康的睡眠习惯,使你更容易入睡。尽量在每天的相同时间上床,并在同一时间起床。
2. 创造一个舒适的睡眠环境:确保睡眠环境舒适,安静,黑暗且温度适宜。可以使用舒适的床上用品,并保持房间通风。
3. 放松身心:在睡前做些放松的活动,例如泡个热水澡,听些轻柔的音乐,阅读一些有趣的书籍等,有助于缓解紧张和焦虑,使你更容易入睡。
4. 避免饮用含有咖啡因的饮料:咖啡因是一种刺激性物质,会影响你的睡眠质量。尽量避免在睡前饮用含有咖啡因的饮料,例如咖啡,茶和可乐。
5. 避免在床上做与睡眠无关的事情:在床上做些与睡眠无关的事情,例如看电影,玩游戏或工作等,可能会干扰你的睡眠。
6. 尝试呼吸技巧:深呼吸是一种放松技巧,可以帮助你缓解紧张和焦虑,使你更容易入睡。试着慢慢吸气,保持几秒钟,然后缓慢呼气。

如果这些方法无法帮助你入睡,你可以考虑咨询医生或睡眠专家,寻求进一步的建议。
```

### 加速对比
| Model         | ROUGE-1  | ROUGE-2  | ROUGE-L  |
|---------------|----------|----------|----------|
| GLM-10B       | **44.7** | 21.4     | **41.4** |
| T5-11B        | 43.5     | **21.6** | 40.7     |
| PEGASUS-Large | 44.2     | 21.5     | **41.4** |
| BART-Large    | 44.2     | 21.3     | 40.9     |


## 协议

本仓库的代码依照 [Apache-2.0](LICENSE) 协议开源

