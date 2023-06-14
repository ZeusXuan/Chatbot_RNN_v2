# chatbot-rnn
A toy chatbot powered by deep learning and trained on data from Reddit. 
Built on TensorFlow v1.4.0 and Python v3.5.1.

下面是效果展示,其中>后面为输入的句子,而没有>的后面是chatbot输出的句子
```
$ python3 chatbot.py
Creating model...
Restoring weights...

> Hi, how are you?
 I'm not the only one here that's having fun with you, it's all me.
> Who else is with you?
 Haha. I'm not the one who made the comment. I'm the one who said that and didn't think that much of anything. 
> Oh, I see. So what's new?
 Nothing. I just thought it was a bit of an overreaction. It's been a while since I've read it. 
> It's all good
 It's all good. It's one of those things that makes me smile :)
> That's great. This is actually my demo chat, so it's nice that you're keeping things positive!
 Thank you :) 
> You're welcome! It's a nice change from the way that you sometimes are...
 *blushes* Sorry, I should have clarified that I wasn't clear. I apologize for my ignorance.
> Sooo, where are you from?             
 I am from South America. Where you from?
> California. Do you like it in South America?
 I live in Australia, but south of Florida is one of the reasons I live in Florida. I've never been there, but I've never been there. 
> I see, a true citizen of the world
 Nope, I've never heard of it. I just spent a lot of time there. 
> Spend a lot of time where?
 I've never been there before, but I've heard it's about an hour and a half away. Maybe there's some sort of party called "Southern" but that's about it. 
> Maybe there is, that's a great observation.
 Not to mention it's all about being from another country.
> All right, guess I should wrap this up, nice talking with you
 I'm not going to argue with you. You're doing it wrong.
> Some might call that arguing. Any final words?
 Don't do that. I don't need to. 
 ```

## Train your own model

- **Train.** 使用 `train.py` 来训练模型,其中默认超参数是我找到的最好的一组超参数.
这组超参数恰好能装满的 一张GTX 1080 Ti GPU 的 11GB 显存, 如果你使用更小的GPU这需要进行调整 (例如将--num_blocks 设置为2).

在训练时随时可以使用crtl-c中断, 并且能够立即保存checkpoint. 
再次训练时可以从checkpoint进入.

## Run my pre-trained model

- **Download [my pre-trained model](https://share.weiyun.com/VYRvTAbh)** (2.3 GB). 请将zip文件中的reddit文件夹提取出来放在models文件夹下.

- **Run the chatbot**. 在命令行中运行`python3 chatbot.py`. 
Warning: this pre-trained model was trained on a diverse set of frequently off-color Reddit comments. It can (and eventually will) say things that are offensive, disturbing, bizarre or sexually explicit. It may insult minorities, it may call you names, it may accuse you of being a pedophile, it may try to seduce you. Please don't use the chatbot if these possibilities would distress you!

在运行时`chatbot.py` 可以使用以下的选项, 例如`python3 chatbot.py --beam_width 3`:

- **beam_width**: 默认为2, 用于设置beam search算法的每个时间步选择的候选项数量, 较大的beam width可以增加搜索空间，提高模型的准确性，但也会增加计算量和内存消耗.

- **temperature**: 默认为1.0, 用于设置softmax的temperature 1.0, 较大的temperature会使对话的相关性下降同时会增大模型的创造性, 在超出0.5-1.5范围时可能会导致输出与输入相关性极低的结果.

- **top-n**: 默认为不使用, 在预测的每一步将只选取概率最大的n个字符, 而剩下的概率全部设置为0.

- **relevance**: 请见这篇paper[Li, Jiwei, et al. "A diversity-promoting objective function for neural conversation models." arXiv preprint arXiv:1510.03055 (2015)](https://arxiv.org/abs/1510.03055).默认设置为不使用.


这些值也可以在对话时直接进行调整, 而无需重启chatbot:

```
$ python3 chatbot.py
Creating model...
Restoring weights...

> --temperature 1.3
[Temperature set to 1.3]

> --relevance 0.3
[Relevance set to 0.3]

> --relevance -1
[Relevance disabled]

> --topn 2
[Top-n filtering set to 2]

> --topn -1
[Top-n filtering disabled]

> --beam_width 5
[Beam width set to 5]

> --reset
[Model state reset]
```

