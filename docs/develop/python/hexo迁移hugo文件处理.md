
# hexo 迁移 hugo 文件处理

hugo 的 md 文件中 categories 和 tags 必须是数组，所以要处理一下。

cd 到 hexo 的 source 文件夹下，创建 hugo 文件夹，执行以下脚本。

```python
#!/usr/bin/python3
# -*- coding: UTF-8 -*- 

import os

path = './_posts/'
files = os.listdir(path)

for file in files:
    f = open(path + file)
    fw = open('./hugo/' + file, "w")
    print(f.name)

    lineList = f.readlines()
    s1 = 'categories: '
    s2 = 'tags: '
    
    s11 = 0
    s22 = 0
    
    for line in lineList:
        strLen = len(line) # len=7
        
        if s1 in line and s11==0:
            line = line.replace('categories: ', 'categories: \n    - ')
            s11=1
        elif s2 in line and s22==0 and strLen>7:
            line = line.replace('tags: ', 'tags: \n    - ')
            s22=1
        fw.write(line)
print('completed...')
```