# 流程

1.加载20类新闻数据，并进行分割

2.生成文章特征词

3.朴素贝叶斯estimator流程进行预估

# 代码

```python
from sklearn.naive_bayes import MultinomialNB
from sklearn.datasets import fetch_20newsgroups
from sklearn.model_selection import train_test_split
from sklearn.feature_extraction.text import TfidfVectorizer
def naviebayes():
    """
    朴素贝叶斯进行文本分类
    :return:None
    """
    # 加载新闻数据集
    news = fetch_20newsgroups(subset='all')
    # 进行数据分割
    x_train, x_test, y_train, y_test = train_test_split(news.data, news.target, test_size=0.25)
    # 对数据集进行特征抽取
    tf = TfidfVectorizer()
    # 以训练集当中的词的列表进行每篇文章重要性统计
    x_train = tf.fit_transform(x_train)
    x_test = tf.transform(x_test)
    # 进行朴素贝叶斯算法的预测
    mlt = MultinomialNB(alpha=1.0)
    mlt.fit(x_train, y_train)
    y_predict = mlt.predict(x_test)
    print('预测结果:', '\n', y_predict)
    print('准确率为:', '\n', mlt.score(x_test, y_test))
    return None
if  __name__ == "__main__":
     naviebayes()
```

# 运行结果

```python
预测结果: 
 [15  0 13 ... 10  7 11]
准确率为: 
 0.8486842105263158
```

