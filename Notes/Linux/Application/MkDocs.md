
先安装 pip

```bash
sudo apt install python3-pip
```

```
$ pip -V
pip 20.0.2 from /usr/lib/python3/dist-packages/pip (python 3.8)
```

安装主题
```bash
pip install mkdocs-material
```

启动
```bash
nohup mkdocs serve -a 0.0.0.0:3000 > logs/`date +%Y%m%d`.log 2>&1 &
```

