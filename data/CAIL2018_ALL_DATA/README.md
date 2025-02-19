# "中国法研杯"司法人工智能挑战赛数据说明

## 一、简介

法律智能旨在赋予机器阅读理解法律文本与定量分析案例的能力，完成罪名预测、法律条款推荐、刑期预测等具有实际应用需求的任务，有望辅助法官、律师等人士更加高效地进行法律判决。近年来，以深度学习和自然语言处理为代表的人工智能技术取得巨大突破，也开始在法律智能领域崭露头角，受到学术界和产业界的广泛关注。

为了促进法律智能相关技术的发展，在最高人民法院信息中心、共青团中央青年发展部的指导下，中国司法大数据研究院、中国中文信息学会、中电科系统团委联合清华大学、北京大学、中国科学院软件研究所共同举办“2018中国‘法研杯’法律智能挑战赛（[CAIL2018](http://180.76.238.177)）”。挑战赛将提供海量的刑事法律文书数据作为数据集，旨在为研究者提供学术交流平台，推动语言理解和人工智能领域技术在法律领域的应用，促进法律人工智能事业的发展。每年比赛结束后将举办技术交流和颁奖活动。诚邀学术界和工业界的研究者和开发者积极参与该挑战赛！



## 二、任务说明

### 2.1 介绍

* 任务一（罪名预测）：根据刑事法律文书中的案情描述和事实部分，预测被告人被判的罪名；
* 任务二（法条推荐）：根据刑事法律文书中的案情描述和事实部分，预测本案涉及的相关法条；
* 任务三（刑期预测）：根据刑事法律文书中的案情描述和事实部分，预测被告人的刑期长短。

### 2.2 数据介绍

数据集是来自于中国法研杯比赛，来自“[中国裁判文书网](http://wenshu.court.gov.cn/)”公开的刑事法律文书，其中每份数据由法律文书中的案情描述和事实部分组成，同时也包括每个案件所涉及的法条、被告人被判的罪名和刑期长短等要素。

数据集共包括`268万刑法法律文书`，共涉及[202条罪名](meta/accu.txt)，[183条法条](meta/law.txt)，刑期长短包括**0-25年、无期、死刑**。

####2.2.0 文件介绍
* exercise\_contest:
    * data\_train.json: 练习赛训练数据
    * data\_test.json：练习赛测试数据
    * data\_valid.json：练习赛验证集数据
* first\_stage:
    * test.json: 第一阶段正赛测试数据
    * train.json：第一阶段正赛训练数据
* final\_test.json：封闭测试阶段测试数据
* restData/rest\_data.json: 在比赛中未用到的数据

#### 2.2.1 字段及意义
数据利用json格式储存，每一行为一条数据，每条数据均为一个字典。

* **fact**: 事实描述  
* **meta**: 标注信息，标注信息中包括:   
    * **criminals**: 被告(数据中均只含一个被告)  
        * **punish\_of\_money**: 罚款(单位：元)
            * **accusation**: 罪名  
                * **relevant\_articles**: 相关法条  
                    * **term\_of\_imprisonment**: 刑期  
                            刑期格式(单位：月)
                                    * **death\_penalty**: 是否死刑  
                                            * **life\_imprisonment**: 是否无期
                                                    * **imprisonment**: 有期徒刑刑期

```
这里是简单的一条数据展示:  
{   
        "fact": "2015年11月5日上午，被告人胡某在平湖市乍浦镇的嘉兴市多凌金牛制衣有限公司车间内，与被害人孙某因工作琐事发生口角，后被告人胡某用木制坐垫打伤被害人孙某左腹部。经平湖公安司法鉴定中心鉴定：孙某的左腹部损伤已达重伤二级。",   
            "meta": 
                {  
                    "relevant_articles": [234],  
                    "accusation": ["故意伤害"], 
                    "criminals": ["段某"],  
                    "term_of_imprisonment": 
                            {  
                                "death_penalty": false,  
                                "imprisonment": 12,  
                                "life_imprisonment": false
                            }
                }
}
```


### 2.3 基线系统

竞赛组织方提供了一个开源的针对不同任务的基线系统（[LibSVM](https://github.com/thunlp/CAIL2018/tree/master/baseline)）。
