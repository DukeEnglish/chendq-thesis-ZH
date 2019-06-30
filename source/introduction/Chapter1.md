# Chapter 1 Introduction

## 1.1  Motivation

教机器理解人类语言文档是人工智能领域最难以捉摸和长期存在的挑战之一。在我们继续往下读之前，我们必须搞清楚，理解人类语言意味着什么？图1.1展示了MCTEST数据集中的一个儿童故事（Richardson et al.，2013），这个故事仅仅使用了简单的词汇和语法。为了处理这样一段文本，NLP社区花了几十年的时间来解决文本理解各个方面的不同任务，包括:

1. 词性标注（part-of-speech tagging）。这需要我们的机器理解文本中的单词词性。例如，第一句话中Alyssa got to the beach after a long trip。其中Alyssa是一个专有名词（proper noun），beach和trip是常见名词（common noun），got是动词的过去式（verb in its past tense），long是形容词（adjective），after是介词（preposition）。

2. 命名实体识别（named entity recognition）。我们的机器也应该明白，故事中是Alyssa、Ellen、Kristen人物的名字，而Charlotte、Atlanta和Miami是地点的名字。

3. 语法解析（syntactic parsing）。为了理解每个句子的意思，我们的机器还需要理解单词之间的关系，或者语法（grammatical）结构。还是用故事中的第一句话举例：Alyssa got to the beach after a long trip，，机器应该理解Alyssa是主语，beach是动词got的宾语，而after a long trip作为一个整体是一个介词短语，它描述了动词与时间的关系。

4. 指代消解。此外，我们的机器甚至需要理解句子之间的关系。例如，She 's now in Miami这句话中提到的She指的是第一句中提到的Alyssa，而提到的The girls指的是前面句子中提到的Alyssa、Ellen、Kristen和Rachel。

   ![Figure1.1: AsamplestoryandcomprehensionquestionsfromtheMCTESTdataset ](/Users/ljy/Library/Application Support/typora-user-images/image-20190630112958669.png)

Figure1.1: A sample story and comprehension questions from the MCTEST dataset
（Richardson et al., 2013）.

是否有一种综合评价可以检验所有这些方面，并探索更深层次的理解？我们认为，阅读理解的任务——在一篇文章中回答理解性问题——是一种恰当而重要的方法。就像我们使用阅读理解测试来衡量一个人对一段文字的理解程度一样，我们相信它也可以在评估计算机系统对人类语言的理解程度方面发挥同样的作用。【译者注，这个和我们利用阅读理解来判定一个人的语言水平是类似的思想，所以高考的阅读理解是很有意义的】

让我们仔细看看同一篇文章中提出的一些理解性问题（Figure 1.1）:

1. 为了回答第一个问题:Alyssa在哪个城市？，我们的机器需要找出she is now in Miami，并解决指代消解的问题，即she在这里指的是Alyssa，最后给出正确答案Miami。
2. 对于第二个问题，What did Alyssa eat at the restaurant? ， 首先，他们需要找到这两句话：The restaurant had a special on catfish（鲶鱼）. 以及 Alyssa enjoyed the restaurant’s special. 并且理解Alyssa在第二句话中所enjoyed的special是第一句话中的special。而基于catfish修饰special，所以答案就是catfish。
3. 最后一个问题特别具有挑战性。为了得到正确的答案，机器必须记住文本中提到的所有人的名字和他们之间的关系，进行一些算术推理（计数），最后给出一个答案。

正如我们所看到的，我们的计算机系统必须理解文本的各个方面来正确地回答这些问题。由于问题可以被设计成问询我们关心的方面，所以**阅读理解可能是评估语言理解最合适的任务**（*reading comprehension could be the most suitable task for evaluating language understanding）*。这是本文的中心主题。

在本文中，我们研究了阅读理解的问题：我们如何构建计算机系统来阅读一篇文章并回答这些理解性问题？我们特别关注**神经阅读理解**（*neural reading comprehension*），这是一种使用深度神经网络建立的阅读理解模型，它已经被证明比非神经（网络）的、基于特征的模型更有效。

阅读理解这个领域有着悠久的历史——早在20世纪70年代，研究者就已经认识到它是测试计算机程序语言理解能力的重要方法（Lehnert， 1977）。然而，这个领域已经被忽视了几十年。直到最近，它才得到了大量的关注，并取得了快速的进展（参见Figure 1.2作为一个例子），包括我们将在本文中详细介绍的我们付出的努力。最近阅读理解的成功可以归因于两个原因：1）以（文章、问题、答案）三元组的形式的大规模监督数据集创建；2）神经阅读理解模型的建立。

![image-20190630114952839](/Users/ljy/Library/Application Support/typora-user-images/image-20190630114952839.png)

Figure 1.2: A search result on GOOGLE. It not only returns a list of search documents but gives more precise answers within the documents. 

 

在这篇论文中，我们将涵盖现代神经阅读理解的本质：问题的形成，系统的构建模块和关键成分，以及理解当前的神经阅读理解系统在哪些方面可以做得更好，哪些方面仍然落后。

本文的第二个中心主题是，我们深信，如果我们能够构建出高性能的阅读理解系统，**它们将成为问答和对话系统等应用的关键技术**（*they would be a crucial technology for applications such as question answering and dialogue systems*）。事实上，这些语言技术已经与我们的日常生活息息相关。例如，如果我们今天在谷歌中输入一个搜索查询“how many people work at stanford univerisity？””（Figure 1.2），谷歌不仅返回搜索文档列表，还尝试读取这些Web文档，最后突出显示最可信的答案，并将它们显示在搜索结果的顶部。我们相信这正是阅读理解可以起作用的地方，因此可以促进更智能的搜索引擎。此外，随着亚马逊（Amazon）的ALEXA、苹果（Apple）的SIRI、谷歌ASSISTANT或微软（Microsoft）的小娜（CORTANA）等数字个人助理的发展，越来越多的用户通过询问有关信息的问题来参与到这些设备（的建设中）[^1]。我们相信，建造能够阅读和理解文本的机器将会极大的改善这些个人助理的能力。

因此，在本文中，我们也对从最近神经阅读理解的成功中构建实际应用感兴趣。我们探索了两个以神经阅读理解为核心的研究方向:

**开放领域的问答**（Open-domain question answering）结合了来自信息检索和阅读理解的挑战，旨在回答来自Web或大型百科全书（如Wikipedia）的一般问题。

**会话形式问答**（Conversational question answering）结合了来自对话和阅读理解的挑战，解决了在一段文本中进行多轮问答的问题，比如用户如何与会话代理进行交互。Figure 1.3展示了来自COQA数据集的一个示例（Reddy et al.，2019）。在这个例子中，一个人可以基于一篇CNN的文章问出一系列的互相关联的问题。

[^1]: A recent study https://www.stonetemple.com/digital-personal-assistants-study/ reported that asking general questions is indeed the number one use for such digital personal assistants.

## 1.2 Thesis Outline

根据我们刚刚讨论的两个中心主题，本文由两部分组成:第一部分神经阅读理解基础（PART I NEURAL READING COMPREHENSION: FOUNDATIONS）和第二部分神经阅读理解应用（PART II NEURAL READING COMPREHENSION: APPLICATIONS）。

【译者注： excruciatingly： 难以忍受地； allegedly：据说】

![image-20190630115625137](/Users/ljy/Library/Application Support/typora-user-images/image-20190630115625137.png)

Figure1.3: AconversationfromCOQAbasedonanCNNarticle. 

第一部分（PART 1）侧重于阅读理解的任务，强调仔细阅读一小段，使计算机系统能够回答理解性问题。

第二章首先概述了阅读理解的发展历史和现状。接下来，我们正式定义了问题的表达式及其表达式的主要类别。然后简要讨论了阅读理解和一般问答（问题）的差异。最后，我们认为最近神经阅读理解的成功是由大规模数据集和神经模型（一起）驱动的。

在第三章中，我们介绍了神经阅读理解模型家族。我们首先描述非神经的，基于特征的分类器，并讨论他们如何不同于端到端神经方法。然后我们介绍了一种我们提出的名叫THE STANFORD ATTENTIVE READER的神经方法，并且描述了它的基本构建模块和扩展。我们在CNN/DAILY MAIL和SQUAD这两个代表性的阅读理解数据集上展示了实验结果。更重要的是，我们对神经模型进行了深入的分析，以理解这些模型实际上学到了什么（译者注：我觉得这是一个重点）。最后，总结了近年来神经阅读理解模型在不同方面的研究进展。本章基于我们的工作（Chen et al.，2016）和（Chen et al.，2017）。

在第四章中，我们讨论了该领域未来的工作和有待解决的问题。我们首先研究现有模型的错误case，尽管模型在当前基准上的准确性很高。然后，我们根据数据集和模型讨论未来的方向。最后，我们对这一领域的几个重要研究问题进行了复盘，这些问题仍然是悬而未决的，有待未来进一步的研究。

第二部分认为阅读理解是实际应用的一个重要组成部分，如问答系统和会话智能体。详情如下：

**开放领域的问答**                                                                                                                                                                                                                                                      

第五章将开放领域的问答当作是阅读理解的一个应用。探讨了如何将高性能的神经阅读理解系统与有效的信息检索技术相结合，构建新一代的开放领域的问答系统。我们描述了我们构建的一个名为DRQA的系统:它的关键组件以及我们如何为它创建训练数据，然后我们对多个问答（系统）基准进行了综合评估。最后我们讨论了这个系统目前的局限性以及未来的工作。本章基于我们的工作（Chen et al.，2017）。

在第六章中，我们研究了会话问答，即机器必须理解一篇文章，并回答会话中出现的一系列问题。本文首先简要回顾了有关对话的文献，认为会话问答是构建信息检索对话智能体的关键。本文介绍了一种用于构建会话问答系统（**Co**nversational **Q**uestion **A**nswering）的新数据集COQA，它由127k个问题和答案组成，这些问题和答案来自于8k个关于文本段落的讨论。我们对数据集进行了深入的分析，并在会话和神经阅读理解模型的基础上建立了若干竞争模型，并给出了实验结果。最后讨论了未来在这方面的工作。本章基于我们的工作（Reddy et al.， 2019）。

我们最后将在第七章进行总结。

## 1.3 Contributions

本文的贡献总结如下:

1. 我们是最早研究神经阅读理解的团队之一。特别地，我们提出了STANFORD ATTENTIVE READER 模型，该模型在各种现代阅读理解任务中都表现出了优异的表现。
2. 我们努力更好地理解神经阅读理解模式实际上学到了什么，以及解决当前任务需要多大的语言理解深度。我们的结论是，与传统的基于特征的分类器相比，**神经模型更善于学习词汇匹配和释义**，而现有系统的**推理能力仍然相当有限**。
3. 我们开创了将神经阅读理解作为开放领域问题回答的核心组成部分的研究方向，并研究了如何通用化这个案例的模型。特别的，我们在DRQA系统中实现了这个想法，这是一个大型的、基于英语维基百科的真实问题回答系统。
4. 最后，我们着手解决会话问答问题。其中计算机系统需要在对话的上下文（背景）中回答理解性问题，所以每个问题都需要根据它的对话历史来理解。为了解决这个问题，我们提出了COQA challenge，并建立了适合这个问题的神经阅读理解模型。我们认为这是构建会话QA智能体的第一步，也是重要的一步。

 

