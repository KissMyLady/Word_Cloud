Word_Cloud  
====
## 说明  
* 1、功能  
    * 一、将txt文本转词云  
    * 二、可以指定图形模板，生成想要的特定形状词云   
    * 三、速度快

* 2、使用方法  
    * 一、复制粘贴，在IDE里仔细阅读源代码(很简单), 注释有详细说明  
    * 二、在GitHub向我提问  
    * 三、问我(QQ群:36877 9008), 私人小群  

* 3、声明  
    * 自己写的小工具，希望能帮助到各位  
    * 可以用OOP更进一步升级代码，欢迎前来讨论, QQ(群:36877 9008)
    * 可进一步优化升级方案， 例如，在视频截图时，计算型操作更频繁， 在图片合并时，IO操作更频繁，  
       可针对这个特性进行优化升级
-
## 生成词云效果图一览

### 停用词文本  

### 图形模板



## 代码  
```Python
import jieba, random
from wordcloud import WordCloud
from scipy.misc import imread
import matplotlib.pyplot as plt


def main(OPEN_TXT_PATN, STOP_word_PATH, LOAD_MODLOE_PATH, SAVE_PATH):
    try:
        # 打开要要生成的文本
        # txt = open(r'G:\big data folder\数据--集合\bookname\心理学.txt', encoding="utf-8").read()
        with open(OPEN_TXT_PATN, 'r', encoding='utf-8') as f:
            txt = f.read()
        # 加载停用词表
        stopwords = [line.strip() for line in open(STOP_word_PATH, encoding="utf-8").readlines()]
        words = jieba.lcut(txt)
        
        counts = {}
        for word in words:             # 依次迭代， 检查word是否在停用词中
            if word not in stopwords:  # 判断,  是否有停用词表的词, 例如: ‘的’  ‘啊’ ‘哦’
                if len(word) == 1:     # 判断,  一个字的也不要
                    continue
                else:
                    counts[word] = counts.get(word, 0) + 1
                    
        items = list(counts.items())
        items.sort(key=lambda x: x[1], reverse=True)
        
        list_add = []
        for i in range(0, 400):
            word, count = items[i]
            for add in range(count):
                list_add.append("%s" % (word))
            
            # 打乱顺序
            random.shuffle(list_add)
        
        # 整体作为字符串去生成词云
        new_text = ' '.join(list_add)
        
        # 加载词云模板，一般是纯色模板
        pac_mask = imread(LOAD_MODLOE_PATH)
        wc = WordCloud(font_path='simhei.ttf',
                       background_color="white",
                       max_words=2000,
                       mask=pac_mask).generate_from_text(new_text)
        plt.imshow(wc)
        plt.axis("off")
        # plt.show()
        wc.to_file(SAVE_PATH)  # 词云图保存地址
        print('---------complete---------')
    except:
        print('请重新检查，有地方错了, 大概率是路径')

if __name__ == '__main__':
    OPEN_TXT_PATN = r'G:\big data folder\数据--集合\bookname\心理学.txt'
    STOP_word_PATH = r"G:\big data folder\数据--集合\词云\大杂烩停用词.txt"
    LOAD_MODLOE_PATH = r"G:\big data folder\数据--集合\云图模板\圆形空白模板.png"
    SAVE_PATH = r"G:\big data folder\数据--集合\bookname_input\心理学.png"
    main(OPEN_TXT_PATN, STOP_word_PATH, LOAD_MODLOE_PATH, SAVE_PATH)
```
## 如有错误、或是需要加强之处，欢迎各位大佬在GitHun向我指出，非常感谢  
- [问题提出请点这里](https://github.com/KissMyLady/Word_Cloud/issues)
