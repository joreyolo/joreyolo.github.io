1.ValueError: cannot reindex from a duplicate axis<br>
<http://codingdict.com/questions/1184>
```
由于存在重复索引导致无法reindex
解决方法：
#判断是否存在重复索引
s.index.duplicated()

#删除重复索引行
s= s[~s.index.duplicated(keep = 'first')]
or 
s= s.loc[~s.index.duplicated(keep = 'first')]
```
