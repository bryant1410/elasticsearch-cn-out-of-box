elasticsearch-cn-out-of-box
------

为elasticsearch集成一些实用插件以及配置的开箱即用的版本。

Dockerfile [docker-elasticsearch-cn](https://github.com/hangxin1940/docker-elasticsearch-cn)

`docker pull hangxin1940/docker-elasticsearch-cn:v2.1.1`

======

* [elasticsearch](https://www.elastic.co/products/elasticsearch) v2.1.1
* [servicewrapper](https://github.com/elasticsearch/elasticsearch-servicewrapper) 0.90

## 站点插件:
* oob
* [bigdesk](http://bigdesk.org/) v2.5.0  (可用, 但不稳定)
* [head](http://mobz.github.io/elasticsearch-head/)  (可用, 但不稳定)
* [kopf](https://github.com/lmenezes/elasticsearch-kopf) v2.0.0
* [segmentspy](https://github.com/polyfractal/elasticsearch-segmentspy) v0.2.0  (可用, 但不稳定)
* [inquisitor](https://github.com/polyfractal/elasticsearch-inquisitor) v0.1.2  
* [hq](http://www.elastichq.org/) v1.0.0

## 分词插件

* [analysis-smartcn](https://www.elastic.co/guide/en/elasticsearch/plugins/current/analysis-smartcn.html) v2.1.1
* [analysis-mmseg](https://github.com/medcl/elasticsearch-analysis-mmseg) v1.7.0
* [analysis-ik](https://github.com/medcl/elasticsearch-analysis-ik) v1.7.0
* [analysis-stconvert](https://github.com/medcl/elasticsearch-analysis-stconvert) v1.6.1
* [analysis-pinyin](https://github.com/medcl/elasticsearch-analysis-pinyin) v1.5.2

## 其他插件

*  [mapper-attachments](https://www.elastic.co/guide/en/elasticsearch/plugins/master/mapper-attachments.html) v2.1.1

## 为 inquisitor 插件增加自定义分析器的预览等

![puglin_oob2](https://raw.githubusercontent.com/hangxin1940/elasticsearch-cn-out-of-box/master/plugin_inquisitor.png)

## 使用方法

浏览器进入插件 `http://localhost:9200/_plugin/oob`

![puglin_oob](https://raw.githubusercontent.com/hangxin1940/elasticsearch-cn-out-of-box/master/puglin_oob.png)

![puglin_oob2](https://raw.githubusercontent.com/hangxin1940/elasticsearch-cn-out-of-box/master/plugin_oob2.png)


## elasticsearch.yml

```yml

# 集群名
#cluster.name: "cn-out-of-box"
# 节点名
#node.name: "node1"
# 是否有资格成为主节点
#node.master: true
# 是否存储索引数据
#node.data: true
# 默认索引分片数
#index.number_of_shards: 3
# 默认索引副本数
#index.number_of_replicas: 1
# 临时文件存储路路径
#path.work: "/tmp/elasticsearch"
# 日志文件存储路路径
#path.logs:  "/var/log/elasticsearch/logs"
# tcp传输端口
#transport.tcp.port: 9300
# 是否压缩tcp传输数据
transport.tcp.compress: true
# http端口
#http.port: 9200
# 是否开启http服务
#http.enabled: true
# 是否打开多播发现节点
#discovery.zen.ping.multicast.enabled: true

#http.max_content_length: 100mb

# 网关服务用于数据恢复
# 参见 http://blog.csdn.net/codingapple/article/details/17308433

# 本地文件系统
#gateway.type: local

# Settings below control how and when to start the initial recovery process on
# a full cluster restart (to reuse as much local data as possible when using shared
# gateway).

# 设置N个节点启动是进行数据恢复
#gateway.recover_after_nodes: 1

# 初始化恢复进程的超时时间
#gateway.recover_after_time: 5m

# 集群数量, 全部节点启动就会进行数据恢复
#gateway.expected_nodes: 2

# 初始化数据恢复的并发线程数
#cluster.routing.allocation.node_initial_primaries_recoveries: 4

#添加删除节点或负载均衡时并发恢复线程的个数
#cluster.routing.allocation.node_concurrent_recoveries: 2

#设置数据恢复时限制的带宽 , 0为无限制
#indices.recovery.max_bytes_per_sec: 20mb

#限制从其它分片恢复数据时最大同时打开并发流的个数
#indices.recovery.concurrent_streams: 5


# 慢查询日志参数
#index.search.slowlog.threshold.query.warn: 10s
#index.search.slowlog.threshold.query.info: 5s
#index.search.slowlog.threshold.query.debug: 2s
#index.search.slowlog.threshold.query.trace: 500ms

#index.search.slowlog.threshold.fetch.warn: 1s
#index.search.slowlog.threshold.fetch.info: 800ms
#index.search.slowlog.threshold.fetch.debug: 500ms
#index.search.slowlog.threshold.fetch.trace: 200ms

index.version.created: 2010199

# 索引配置
index:

  # 分析配置
  analysis:
  
    # 分词器配置  
    tokenizer:

# ======== analysis-pinyin ========
      # 完整拼音
      my_pinyin:
        type: pinyin
        first_letter: prefix
        padding_char: ' '
        
      # 拼音首字母
      pinyin_first_letter:
        type: pinyin
        first_letter: only

# ======== analysis-mmseg ========
      # 简单正向匹配
      #       example: 一个劲儿的说话
      #       一个
      #       一个劲
      #       一个劲儿
      #       一个劲儿的
#      mmseg_simple:
#        type: mmseg
#        seg_type: simple
        
      # 匹配出所有的“三个词的词组”
      # 并使用四种规则消歧(最大匹配、最大平均词语长度、词语长度的最小变化率、所有单字词词频的自然对数之和)
      #       example: 研究生命起源
      #       研_究_生
      #       研_究_生命
      #       研究生_命_起源
      #       研究_生命_起源
#    mmseg_complex:
#       type: mmseg
#        seg_type: complex
        
      # 基于complex的最多分词
      #       example: 中国人民银行
      #       中国|人民|银行
#      mmseg_maxword:
#        type: mmseg
#        seg_type: max_word

# ======== analysis-stconvert ========
      # 简繁转换，只输出繁体
#      s2t_convert:
#        type: stconvert
#        delimiter: ","
#        convert_type: s2t
#        
     # 繁简转换，只输出简体
#      t2s_convert:
#        type: stconvert
#        delimiter: ","
#        convert_type: t2s

     # 简繁转换，同时输出繁体简体
#      s2t_keep_both_convert:
#        type: stconvert
#        delimiter: ","
#        keep_both: 'true'
#        convert_type: s2t
        
     # 繁简转换，同时输出简体繁体
#      t2s_keep_both_convert:
#        type: stconvert
#        delimiter: ","
#        keep_both: 'true'
#        convert_type: t2s
        
# ======== analysis-pattern ========
     # 正则，分号分词
      semicolon_spliter:
        type: pattern
        pattern: ";"

     # 正则，%分词
      pct_spliter:
        type: pattern
        pattern: "[%]+"
 
 # ======== analysis-nGram ========     
      # 1~2字为一词
      ngram_1_to_2:
        type: nGram
        min_gram: 1
        max_gram: 2

      # 1~3字为一词
      ngram_1_to_3:
        type: nGram
        min_gram: 1
        max_gram: 3

    # 过滤器配置
    filter:

 # ======== ngram filter ========     
      ngram_min_3:
        max_gram: 10
        min_gram: 3
        type: nGram
      ngram_min_2:
        max_gram: 10
        min_gram: 2
        type: nGram
      ngram_min_1:
        max_gram: 10
        min_gram: 1
        type: nGram

 # ======== length filter ========    
      min2_length:
        min: 2
        max: 4
        type: length
      min3_length:
        min: 3
        max: 4
        type: length

 # ======== pinyin filter ========  
      pinyin_first_letter:
        type: pinyin
        first_letter: only
        
    # 分析器配置
    analyzer:
    
      lowercase_keyword:
        type: custom
        filter:
        - lowercase
        tokenizer: standard

      lowercase_keyword_ngram_min_size1:
        type: custom
        filter:
        - lowercase
        - stop
        - trim
        - unique
        tokenizer: nGram

      lowercase_keyword_ngram_min_size2:
        type: custom
        filter:
        - lowercase
        - min2_length
        - stop
        - trim
        - unique
        tokenizer: nGram

      lowercase_keyword_ngram_min_size3:
        type: custom
        filter:
        - lowercase
        - min3_length
        - stop
        - trim
        - unique
        tokenizer: ngram_1_to_3

      lowercase_keyword_ngram:
        type: custom
        filter:
        - lowercase        
        - stop
        - trim
        - unique
        tokenizer: ngram_1_to_3

      lowercase_keyword_without_standard:
        type: custom
        filter:
        - lowercase
        tokenizer: keyword

      lowercase_whitespace:
        type: custom
        filter:
        - lowercase
        tokenizer: whitespace

 # ======== ik  ========    
       # ik分词器
 #     ik:
 #       alias:
#        - ik_analyzer
#        type: ik_smart
        
      # ik最细粒度切分 「中华人民共和国国歌」会被拆分为「中华人民共和国、中华人民、中华、华人、人民共和国、人民、人、民、共和国、共和、和、国国、国歌」
#      ik_max_word:
#        type: custom
#        use_smart: false
        
      # ik智能切分 「中华人民共和国国歌」会被拆分为「中华人民共和国、国歌」
#      ik_smart:
#        type: custom
#        use_smart: true

 # ======== mmseg  ========    
       # mmseg分词器
      mmseg_maxword:
        type: custom
        filter:
        - lowercase
        tokenizer: mmseg_maxword
        
      mmseg_complex:
        type: custom
        filter:
        - lowercase
        tokenizer: mmseg_complex
        
      mmseg_simple:
        type: custom
        filter:
        - lowercase
        tokenizer: mmseg_simple

 # ======== 正则 ======== 
      comma_spliter:
        type: pattern
        pattern: "[,|\\s]+"
        
      pct_spliter:
        type: pattern
        pattern: "[%]+"
        
        
      custom_snowball_analyzer:
        type: snowball
        language: English
        
      simple_english_analyzer:
        type: custom
        tokenizer: whitespace
        filter:
        - standard
        - lowercase
        - snowball
        
      edge_ngram:
        type: custom
        tokenizer: edgeNGram
        filter:
        - lowercase
 
  # ======== 拼音分析 ========        
      pinyin_ngram_analyzer:
        type: custom
        tokenizer: my_pinyin
        filter:
        - lowercase
        - nGram
        - trim
        - unique

  # ======== 拼音首字母分词 ========     
      pinyin_first_letter_analyzer:
        type: custom
        tokenizer: pinyin_first_letter
        filter:
        - standard
        - lowercase
 
   # ======== 拼音首字母分词并过滤 ========
      pinyin_first_letter_keyword_analyzer:
        alias:
        - pinyin_first_letter_analyzer_keyword
        type: custom
        tokenizer: keyword
        filter:
        - pinyin_first_letter
        - lowercase

   # ======== 简繁体 ========
#      stconvert:
#        alias:
#        - st_analyzer
#        type: stconvert
        
#      s2t_convert:
#        type: stconvert
#        delimiter: ","
#        convert_type: s2t
        
#      t2s_convert:
#        type: stconvert
#        delimiter: ","
#        convert_type: t2s
        
#      s2t_keep_both_convert:
#        type: stconvert
#        delimiter: ","
#        keep_both: 'true'
#        convert_type: s2t
        
#      t2s_keep_both_convert:
#        type: stconvert
#        delimiter: ","
#        keep_both: 'true'
#        convert_type: t2s
       
      # 路径分析
      path_analyzer: 
        type: custom
        tokenizer: path_hierarchy
           
      uax_url_email: 
        tokenizer: uax_url_email 
        filter: [standard, lowercase, stop] 
 

# 线程池设置
#threadpool:   
#   index:   
#       type: fixed   
#        size: 30   
#       queue: -1   
#        reject_policy: caller  

```
