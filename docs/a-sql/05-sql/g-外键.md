# 外键与表设计

## 外键约束

外键：用来让两张表的数据之间建立连接，从而保证数据的一致性和完整性。

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/16498880098068_image7.png" style="zoom:80%;" />

对于歌曲的案例，增加歌词表，由于歌词是大段文本，在表设计时没有和歌曲表设计在一起，以提高查询效率。但是，歌词又要和每一首歌曲关联，所以与外键链接。

