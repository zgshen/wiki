
# Flask 读取图片返回前端

根据随机文件名读取图片并返回给前端。

```python
import flask
from flask import Response
import random

app = flask.Flask(__name__)

@app.route("/api", methods=['get'])
def api():
    return '''Self-use API.  
                </br> 
                Use /api/pic to get a backgroud image.'''

@app.route("/api/pic", methods=['post', 'get'])
def index():
    num = random.randint(1, 11)
    
    path = "/root/app/api/comic/comic%s.png" % num

    resp = Response(open(path, 'rb'), mimetype="image/jpeg")
    return resp

app.run(host='0.0.0.0',port=12888, debug=False)
```