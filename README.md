# Familia

[![Build Status](https://travis-ci.org/baidu/Familia.svg?branch=master)](http://travis-ci.org/baidu/Familia)

Familia 开源项目包含文档主题推断工具、语义匹配计算工具以及基于工业级语料训练的三种主题模型：Latent Dirichlet Allocation(LDA)、SentenceLDA 和Topical Word Embedding(TWE)。 支持用户以“拿来即用”的方式进行文本分类、文本聚类、个性化推荐等多种场景的调研和应用。考虑到主题模型训练成本较高以及开源主题模型资源有限的现状，我们会陆续开放基于工业级语料训练的多个垂直领域的主题模型，以及这些模型在工业界的典型应用方式，助力主题模型技术的科研和落地。

# 应用介绍
    
Familia目前包含的三种主题模型对应的论文介绍可以参考[相关论文](https://github.com/baidu/Familia/wiki/Reference "相关论文")。
主题模型在工业界的应用范式可以抽象为两大类: 语义表示和语义匹配。

- **语义表示**
    
    对文档进行主题降维，以获得文档的语义表示，这些表示可以应用于文本分类、文本聚类、CTR预估等下游应用。

- **语义匹配**

    计算文本间的语义匹配度，在代码中我们提供了两种文本类型的相似度计算方式:

    - 短文本-长文本相似度计算，使用场景如计算搜索引擎查询(Query)到文档(Document)的相似度。
    - 长文本-长文本相似度计算，使用场景如衡量两篇文档之间、用户画像和新闻/广告之间的相似度。

# 代码编译
第三方依赖包括gflags-2.0，glogs-0.3.4，protobuf-2.5.0, 同时要求编译器支持C++11, g++ >= 4.8, 兼容Linux和Mac操作系统。
默认情况下执行以下脚本会自动获取依赖并安装。
    
    sh build.sh # 包含获取并安装第三方依赖的过程

# 模型下载

    cd model
    sh download_model.sh

* 关于模型的详细配置说明可以参考[模型说明](https://github.com/baidu/Familia/blob/master/model/README.md)

# 运行DEMO
## 文档主题推断
    
    sh run_inference_demo.sh # 运行文档主题推断的demo
    
执行程序后，通过标准流方式进行输入，每行为一个文档，程序输出每个文档的主题分布。如下所示：

    请输入需要推断主题分布的文档:
    百度又一次展示了自动驾驶领域领导者的大气风范，发布了一项名为“Apollo(阿波罗)”的新计划，向汽车行业及自动驾驶领域的合作伙伴提供一个开放、完整、安全的软件平台，帮助他们结合车辆和硬件系统，快速搭建一套属于自己的完整的自动驾驶系统。
    
    文档主题分布:
    325:0.122222 1319:0.084444 767:0.065556 897:0.064444 1046:0.043333 357:0.037778 1798:0.034444 1600:0.032222 1327:0.030000 863:0.030000 1434:0.028889 1113:0.027778 938:0.025556 1399:0.025556 1771:0.025556 753:0.020000 25:0.020000 1667:0.016667 29:0.016667 649:0.015556 68:0.014444 49:0.011111 1254:0.011111 121:0.010000 887:0.008889 1518:0.007778 127:0.007778 1508:0.007778 1284:0.007778

* 为了便于直观展示，默认输出为稀疏结果（忽略先验的作用），如需获得带先验的稠密主题分布需调用dense_topic_dist函数接口。

## 语义匹配计算

    sh run_semantic_matching_demo.sh # 运行语义匹配计算的demo

程序默认采用短文本与长文本语义匹配计算模式，运行结果如下所示

    请输入短文本:
    百度宣布阿波罗计划 开放自动驾驶技术有望改变汽车产业
    请输入长文本:
    百度又一次展示了自动驾驶领域领导者的大气风范，发布了一项名为“Apollo(阿波罗)”的新计划，向汽车行业及自动驾驶领域的合作伙伴提供一个开放、完整、安全的软件平台，帮助他们结合车辆和硬件系统，快速搭建一套属于自己的完整的自动驾驶系统。
    LDA Similarity = 0.0124648
    TWE Similarity = 0.176046

将脚本中的--mode参数修改为1，程序采用长文本与长文本语义匹配计算模式, 运行结果如下所示

    请输入文档1:
    在人工智能发展得最为系统化的硅谷，AI工程师们的薪水远高于其他领域的同行。随着人工智能概念的不断深入人心，人工智能的人才愈发的紧俏，时至今日，大学刚毕业的博士也能坐拥八九十万的年薪，与资深的硅谷工程师相媲美。
    请输入文档2:
    在国内，部分企业早已瞄准人才的短板，走在了业界的前面。百度是最早进行AI的人才培养布局的，他们同国内诸多高校开展合作，共建工程实验室，在数据开放和资源共享上进行各种合作。这种方式类似美国在人工智能教育领域推行的“硅谷-斯坦福”校企联动模式，一方面斯坦福大学为硅谷提供了人才和科研成果，另一方面硅谷为斯坦福大学提供资金支持和大数据，以助力他们的科研能有更大的突破。
    Jensen Shannon Divergence = 0.0283928
    Hellinger Distance = 0.179698

## 模型内容展现

### 主题内容查询

主题内容查询demo展示三个模型的主题词结果。

#### LDA和SentenceLDA的内容展现

    sh run_show_topic_demo.sh # 查询LDA与SentenceLDA模型的主题词demo

执行程序后，通过标准流方式输入主题id，程序会返回该主题下最重要的K个词，LDA模型内容展现的结果如下所示：

    请输入主题编号(0-1999): 105
    -----------------------------
    对话    0.189676
    合作    0.0805558
    中国    0.0276284
    磋商    0.0269797
    交流    0.021069
    联合    0.0208559
    国家    0.0183163
    讨论    0.0154165
    支持    0.0146714
    包括    0.014198

同理，SentenceLDA模型内容展现结果如下所示：

    请输入主题编号(0-1999): 105
    ------------------------------
    浙江    0.0300595
    浙江省  0.0290975
    宁波    0.0195277
    记者    0.0174735
    宁波市  0.0132504
    长春市  0.0123353
    街道    0.0107271
    吉林省  0.00954326
    金华    0.00772971
    公安局  0.00678163

其中，第一列表示LDA/SentenceLDA模型每个主题下的词，第二列的数值表示词在该主题下的重要程度。
可通过更改脚本中`--item_topic_table_path`来指定LDA/SentenceLDA模型，`--vocabulary_path`配置对应的词表，`--top_k`和`--num_topics`来配置展现结果数以及模型对应的主题数目。

    --vocabulary_path="./model/news/vocab_info.txt" --item_topic_table_path="./model/news/news_lda.model" --top_k=10 --num_topics=2000 # 选用新闻LDA主题模型，展现前10个结果

#### TWE模型的内容展现

    sh run_topic_word_demo.sh # 运行主题词查询的demo

执行程序后，通过标准流方式输入主题id，程序会返回每个主题下最重要的K个词, TWE模型内容展现的结果如下所示：

    请输入主题编号(0-10000):    105
    Embedding Result              Multinomial Result
    ------------------------------------------------
    对话                               对话
    磋商                               合作
    合作                               中国
    非方                               磋商
    探讨                               交流
    对话会议                           联合
    议题                               国家
    中方                               讨论
    对话会                             支持
    交流                               包括

其中，第一列为基于embedding的结果，第二列为基于多项分布的结果，均按照在主题中的重要程度从大到小的顺序排序。
可通过更改脚本中`--work_dir`和`--emb_file`的配置选择其他TWE模型，`--topic_words_file`配置主题模型的主题结果，如

    --work_dir="./model/news/" --emb_file="news_twe_lda.model" --topic_words_file="topic_words.lda.txt" # 选用新闻LDA主题模型训练得到TWE模型以及对应的主题展现结果

### 邻近词查询

    sh run_word_distance_demo.sh # 运行邻近词查询的demo

执行程序后，通过标准流方式进行输入，每行为一个词，程序会输出每个词的最邻近的K个词。如下所示：

    请输入词语:      篮球
    Word           Cosine distance
    ------------------------------
    足球              0.903682
    网球              0.842661
    羽毛球            0.836915
    足球比赛          0.809366
    五人制足球        0.799211
    美式足球          0.791207
    中国足球          0.788995
    乒乓球            0.788278
    五人制            0.784913
    足球新闻          0.783203

其中，每一行为一个词，数字表示该词与输入词的cosine距离，按照从大到小的顺序排序。可通过更改脚本中`--work_dir`配置选择其他模型，`--emb_file`配置选择对应的词向量模型文件，`--top_k`配置展现词的个数，如

    ./word_distance_demo --work_dir="./model/news" --emb_file="news_twe_lda.model" --top_k=20

# 注意事项

*   若出现找不到libglog.so, libgflags.so等动态库错误，请添加third_party至环境变量的`LD_LIBRARY_PATH`中。

    `export LD_LIBRARY_PATH=./third_party/lib:$LD_LIBRARY_PATH`

* 代码中内置简易的FMM分词工具，只针对主题模型中出现的词表进行正向匹配。该工具仅用于Demo示例使用，若对分词和语义准确度有更高要求，建议使用商用分词工具，并使用自定义词表的功能导入主题模型中的词表。

