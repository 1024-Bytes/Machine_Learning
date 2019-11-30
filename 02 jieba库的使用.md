# jieba库常用函数：

## jieba库分词的三种模式

1.精准模式：把文本精准的分开 ，不存在冗余

2.全模式：把文本中所有可能的词语都扫描出来，存在冗余

3.搜索引擎模式：在精准模式的基础上，再次对长词进行切分

## 不同模式的函数

|             函数             | 描述                                         |
| :--------------------------: | -------------------------------------------- |
|        `jieba.cut(s)`        | 精确模式，返回一个可迭代的数据类型           |
| `jieba.cut(s,cut_all=True)`  | 全模式，输出文本s中所有可能单词              |
|  `jieba.cut_for_search(s)`   | 搜索引擎模式，适合搜索引擎建立索引的分词结果 |
|       `jiebal.cut(s)`        | 精确模式，返回一个列表类型                   |
| `jieba.lcut(s,cut_all=True)` | 全模式，返回一个列表类型                     |
|  `jieba.lcut_for_search(s)`  | 搜索引擎模式，返回一个列表类型               |
|     `jieba.add_word(w)`      | 向分词词典中添加新词                         |

# jieba库使用实例

## 精准模式：

代码：

```python
import jieba
print(jieba.lcut("中国是一个伟大的国家"))
```

运行结果：

```python
Building prefix dict from the default dictionary ...
Loading model from cache C:\Users\win10\AppData\Local\Temp\jieba.cache
['中国', '是', '一个', '伟大', '的', '国家']
Loading model cost 0.902 seconds.
Prefix dict has been built succesfully.
```

## 全模式：

代码：

```python
import jieba
print(jieba.lcut("中国是一个伟大的国家",cut_all=True))
```

运行结果：

```python
Building prefix dict from the default dictionary ...
Loading model from cache C:\Users\win10\AppData\Local\Temp\jieba.cache
Loading model cost 0.901 seconds.
['中国', '国是', '一个', '伟大', '的', '国家']
Prefix dict has been built succesfully.
```

## 搜索引擎模式：

代码：

```python
import jieba
print(jieba.lcut_for_search("中华人民共和国是伟大的"))
```

运行结果：

```python
Building prefix dict from the default dictionary ...
Loading model from cache C:\Users\win10\AppData\Local\Temp\jieba.cache
Loading model cost 0.917 seconds.
['中华', '华人', '人民', '共和', '共和国', '中华人民共和国', '是', '伟大', '的']
Prefix dict has been built succesfully.
```

## 向分词词典增加新词：

代码：

```python
import jieba
jieba.add_word("蟒蛇语言")
print(jieba.lcut("python是蟒蛇语言"))
```

运行结果：

```python
Building prefix dict from the default dictionary ...
Loading model from cache C:\Users\win10\AppData\Local\Temp\jieba.cache
Loading model cost 0.916 seconds.
Prefix dict has been built succesfully.
['python', '是', '蟒蛇语言']
```

## 统计字符及频率：

代码：

```python
import jieba
s='我有一只小毛驴我从来也不骑，有一天我心血来潮骑着去赶集，我手里拿着小皮鞭我心里正得意，不知怎么哗啦啦啦我摔了一身泥 '
words=jieba.lcut(s)
for word in set(words):
    n=words.count(word)
    print({word:n})
```

**注**：set()方法可以去除列表中重复元素，count()可以统计元素在相应列表中出现次数。

运行结果：

```python
Building prefix dict from the default dictionary ...
Loading model from cache C:\Users\win10\AppData\Local\Temp\jieba.cache
{'拿': 1}
{'骑': 1}
{'一身': 1}
{'得意': 1}
{'骑着': 1}
{'小毛驴': 1}
{'有': 2}
{'啦': 1}
{'，': 3}
{'不': 1}
{'心里': 1}
{'我': 6}
{'正': 1}
{' ': 1}
{'不知': 1}
{'赶集': 1}
{'小': 1}
{'泥': 1}
{'一只': 1}
{'心血来潮': 1}
{'皮鞭': 1}
{'从来': 1}
{'怎么': 1}
{'一天': 1}
{'去': 1}
{'摔': 1}
{'哗啦啦': 1}
{'着': 1}
{'手里': 1}
{'也': 1}
{'了': 1}
Loading model cost 1.014 seconds.
Prefix dict has been built succesfully.
```

##  **查找出“threekingdoms.txt”文件中出现频率前十位的词汇**  ：

代码：

```python
import jieba
txt = open("threekingdoms.txt", "r", encoding='utf-8').read()
words  = jieba.lcut(txt)
counts = {}
for word in words:
    if len(word) == 1:
        continue
    else:
        counts[word] = counts.get(word,0) + 1
items = list(counts.items())
items.sort(key=lambda x:x[1], reverse=True) 
for i in range(15):
    word, count = items[i]
    print ("{0:<10}{1:>5}".format(word, count))
```

**注**：`dict.get(key,s)`表示字典中相应key值不存在时,就会返回s,`list.sort(key=lambda x:x[1],reverse=True)`表示按照列表list中元素的第二维数组进行排序，这里x为对象，x[1]表示对象的第二维数据，reverse=True表示按从大到小排列(反向排序开启)

运行结果：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190426152958725.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg2NjU2Nw==,size_16,color_FFFFFF,t_70)  

## 人物出场统计：

代码：

```python
import jieba
excludes = {"将军","却说","荆州","二人","不可","不能","如此"}
txt = open("threekingdoms.txt", "r", encoding='utf-8').read()
words  = jieba.lcut(txt)
counts = {}
for word in words:
    if len(word) == 1:
        continue
    elif word == "诸葛亮" or word == "孔明曰":
        rword = "孔明"
    elif word == "关公" or word == "云长":
        rword = "关羽"
    elif word == "玄德" or word == "玄德曰":
        rword = "刘备"
    elif word == "孟德" or word == "丞相":
        rword = "曹操"
    else:
        rword = word
    counts[rword] = counts.get(rword,0) + 1
for word in excludes:
    del counts[word]
items = list(counts.items())
items.sort(key=lambda x:x[1], reverse=True) 
for i in range(10):
    word, count = items[i]
    print ("{0:<10}{1:>5}".format(word, count))
```

运行结果：

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190426152917630.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg2NjU2Nw==,size_16,color_FFFFFF,t_70) 

