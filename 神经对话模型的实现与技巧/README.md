
# Sequence-to-Sequence 模型

流程伪代码（python）

训练时：

```python
input_text = ['A', 'B', 'C']

output_text = ['D', 'E', 'F']

output_text_with_start = ['<SOS>'] + output_text

output = []
for decoder_input in output_text_with_start:
    decoder_output = decoder(encoder_state, decoder_input)
    output.append(decoder_output)

"""
decoder(encoder_state, '<SOS>') -> 'D'
decoder(encoder_state, 'D') -> 'E'
decoder(encoder_state, 'E') -> 'F'
decoder(encoder_state, 'F') -> '<EOS>'
output == ['D', 'E', 'F', '<EOS>']
"""
```

预测时

```python
input_text = ['A', 'B', 'C']

output_text = ['D', 'E', 'F']

last_decoder_output = '<SOS>'
output = []
while True:    
    decoder_output = decoder(encoder_state, last_decoder_output)
    output.append(decoder_output)

    if decoder_output == '<EOS>':
        break

    if len(output) > output_length_limit:
        break

"""
decoder(encoder_state, '<SOS>') -> 'D'
decoder(encoder_state, 'D') -> 'E'
decoder(encoder_state, 'E') -> 'F'
decoder(encoder_state, 'F') -> '<EOS>'
output == ['D', 'E', 'F', '<EOS>']
"""
```

# 与神经机器翻译的异同

## embedding不同

## 结果不同

神经机器翻译往往是一对一的映射，一句话对应一句话，例如：

hello <> 你好  
bye <> 再见  

也就是说输入与输出都是唯一对应的输出，或者近似对应的输出，虽然也可能一句话、或者一个词有多个翻译：

hi <> 你好  
hello <> 你好

但是这种情况并不会太多，而对话模型这种问题更普及，例如可以：

你好吗？ > 烦着呢别理我  
今天我们去哪玩？ > 烦着呢别理我  
我们去吃牛排吧 > 烦着呢别理我   
你知道qhduan吗 > 不知道  
你知道qhduan是帅哥吗 > 不知道  
你知道龙傲天吗 > 不知道  

这也导致对话模型更容易出现“I don't know”问题，
也就是模型更容易学会更好回答而不犯错的答案，任何问题都回答“我不知道”，或者类似的简单回复。

所以后续的一些文献更看重神经对话模型的diversity，也就是结果多样。

## 对称性不同

神经翻译模型有对称性，例如

en_zh(hello) = 你好

zh_en(你好) = hello

即有：

zh_en(en_zh(hello)) = hello

也就是平行语料之间有对称性，但是聊天的上一句下一句是没有的，不能说：

chat(你是谁) = 我是你哥哥

chat(我是你哥哥) != 你是谁

## 语料问题

## 评价标准问题

# 技巧与提高（trick）

互信息技巧

反转技巧

强化学习

对抗学习

TFIDF技巧

上下文相似技巧
