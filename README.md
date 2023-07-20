## Dataset

Our dataset in 'https://github.com/WeilaiFuture/CEDG-QA/tree/main/CEDGQA/src/main/resources/datasets'

    -aiwen.jsonl
    -sougou.jsonl
    -zhihu.jsonl


# EDGQA

## What is EDGQA?

EDGQA is a QA system over knowledge bases based on Entity-Description Graphs (EDGs).


## 1. Requirements

- JDK 1.8.0
- Maven
- python 3.6

Knowledge base dumps and linking systems are also needed.

### 1.1 Knowledge Base//修改

CNDBPedia in 'CEDG-QA/CQL'
CKGG in 'CEDG-QA/CKGG-main','https://zenodo.org/record/4668711'

In EDGQA, `DBpedia 1604` (for lcquad) and `DBpedia 1610` (for qald-9) is stored in `Virtuoso`. 

You can deploy the dbpedia locally or use the online endpoint (can be incompatible to the datasets). 
Then fill the server address and port in `src/main/java/cn/edu/nju/ws/edgqa/utils/kbutil/KBUtil.java`.

- [DBpedia 1604](http://downloads.dbpedia.org/2016-04/)
- [Virtuoso](http://vos.openlinksw.com/owiki/wiki/VOS/VOSDownload)

### 1.2 Linking tools

`Earl`, `Falcon`, `Dexter` are used in EDGQA. 

See directory [linking_tools](https://github.com/HXX97/EDGQA/tree/main/linking_tools), and follow the instructions to set up the three linking systems.
Then fill in the server address and port in 
`src/main/java/cn/edu/nju/ws/edgqa/utils/linking/LinkingTool.java`.

For more information:
- [Dexter](https://github.com/dexter/dexter)
- [EARL](https://github.com/AskNowQA/EARL)
- [Falcon](https://github.com/AhmadSakor/falcon)

### 1.3 Semantic matching models
EDGQA employs bert-based classifier as semantic matching models for relation detection and query reranking.

See directory [models](https://github.com/HXX97/EDGQA/tree/main/models) to deploy the models correctly. 


## 2. Run QA

### 2.1 Run EDGQA

Program arguments are defined in `src/main/java/cn/edu/nju/ws/edgqa/main/QAArgs.java`.

Running settings for [Intellij IDEA 2019.3 above versions](https://www.jetbrains.com/idea/) are stored in `EDGQA/.run`.

Run `src/main/java/cn/edu/nju/ws/edgqa/main/EDGQA.java` by following CLI arguments:

```text
-d --dataset: 'lc-quad', 'qald-9'，'Geo'
-tr --train: 'true' for training set, 'false' for test set
-r --run: 'autotest', 'single', or 'serial_number'
-uc --use_cache: 'true' for using linking cache, 'false' otherwise
-cc --create_cache: 'true' for creating linking cache, 'false' otherwise
-gll --global_linking: 'true' for using global linking, 'false' otherwise
-lll --local_linking: 'true' for using local linking, 'false' otherwise
-qd --question_decomposition: 'true' for using EDG to decompose the question
-rr --reranking: 'true' for re-ranking by EDG block, 'false' otherwise
```

Because the linking tools consume a lot of time, caching the linking results of the test queries helps improve the speed
of the test. The cache needs to be built the first time the QA is run and is available when it is run again. Use the
arguments `use_cache` and `create_cache` above to set the cache tool.

## 3. Resources

- [PKUmod paraphrase dict](https://github.com/pkumod/Paraphrase/blob/master/dic.txt)
- [BERT by Google Research](https://github.com/google-research/bert)
- [java-string-similarity](https://github.com/rrice/java-string-similarity)

