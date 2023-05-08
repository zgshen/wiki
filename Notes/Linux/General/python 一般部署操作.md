
安装 pip 
```bash
apt install python3-pip
```

验证
```
pip3 --version
```

安装虚拟环境
```bash
pip3 install virtualenv
```

创建运行环境
```bash
virtualenv [虚拟环境名称] 
virtualenv venv

#如果不想使用系统的包,加上–no-site-packeages参数
virtualenv  --no-site-packages 创建路径名
```


>> 创建并激活虚拟环境 (不知道跟上面区别)
```
python3 -m venv venv
source venv/bin/activate
```

要删除一个虚拟环境，只需删除它的文件夹。(执行 rm -rf venv )。

导出当前环境所有的外部库
```bash
生成 requirements.txt 文件 
pip freeze >> requirements.txt
```


根据 requirements.txt 安装依赖
```bash
pip3 install -r requirements.txt
```

执行 python 脚本
```bash
python3 script.py

# 后台运行
nohup python3 script.py >/dev/null 2>&1 &
```


import 和 from...import

1. import A
A为模块名，直接使用import导入整个模块A，调用模块A其中的某个方法、变量B使用以下形式：  
A.B

2. from A import B
A为模块名，B为模块A中的某个类、方法或者变量等，调用B直接使用以下形式：  
B
