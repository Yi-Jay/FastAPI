# FastAPI

##### FastAPI 是一个用于构建 API 的现代、快速（高性能）的 web 框架

###### 特点：快速、高效编码、更少bug、智能、简单、简短、健壮、标准化

###### 依赖：python3.8+

### 安装

~~~python
pip install fastapi
ASGI服务器生产环境可以使用uvicorn或hypercorn：
pip install "uvicorn或[standard]"
~~~

### 运行

###### 创建一个main.py文件

###### 添加内容

~~~py
from typing import Union
from fastapi import FastAPI
app = FastAPI()

@app.get("/")
def read_root():
    return {"Hello": "World"}

@app.get("/items/{item_id}")
def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}
~~~

###### 如果代码出现saync/await使用以下

~~~py
from typing import Union
from fastapi import FastAPI
app = FastAPI()

@app.get("/")
async def read_root():
    return {"Hello": "World"}

@app.get("/items/{item_id}")
async def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}
~~~

###### 运行命令

~~~py
uvicorn main:app --reload
~~~

http://127.0.0.1:8000/

显示：{"Hello":"World"}

http://127.0.0.1:8000/items/5?q=somequery

显示：{"item_id": 5, "q": "somequery"}

##### 以上代码拆分

1、导入模块和类

~~~py
from typing import Union
from fastapi import FastAPI
~~~

2、创建实例

~~~py
app = FastAPI()
~~~

3、定义根路径

~~~py
@app.get("/")
async def read_root():
    return {"Hello": "World"}
~~~

4、定义带路径参数和查询参数的路由

~~~py
@app.get("/items/{item_id}")
def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}
~~~

这个路由操作使用了 **@app.get("/items/{item_id}")** 装饰器，表示当用户通过 **HTTP GET** 请求访问 **/items/{item_id}** 路径时，将执行 **read_item** 函数。

函数接受两个参数：

- **item_id** --是路径参数，指定为整数类型。
- **q** -- 是查询参数，指定为字符串类型或空（None）。

函数返回一个字典，包含传入的 item_id 和 q 参数。

q 参数通过 Union[str, None] 表示可以是字符串类型或空，这样就允许在请求中不提供 q 参数。



## 交互式API文档

修改main.py文件来从 `PUT` 请求中接收请求体

~~~py
from typing import Union

from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    price: float
    is_offer: Union[bool, None] = None


@app.get("/")
def read_root():
    return {"Hello": "World"}


@app.get("/items/{item_id}")
def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}


@app.put("/items/{item_id}")
def update_item(item_id: int, item: Item):
    return {"item_name": item.name, "item_id": item_id}
~~~

安装swagger-ui：

~~~py
pip install swagger-ui
~~~

http://127.0.0.1:8000/docs 

- 交互式 API 文档将会自动更新，并加入新的请求体：

![image-20240526214344039](C:\Users\often\AppData\Roaming\Typora\typora-user-images\image-20240526214344039.png)

- 点击「Try it out」按钮，之后你可以填写参数并直接调用 API：

![image-20240526214434866](C:\Users\often\AppData\Roaming\Typora\typora-user-images\image-20240526214434866.png)

- 然后点击「Execute」按钮，用户界面将会和 API 进行通信，发送参数，获取结果并在屏幕上展示：

![image-20240526214553620](C:\Users\often\AppData\Roaming\Typora\typora-user-images\image-20240526214553620.png)



http://127.0.0.1:8000/redoc

- 可选文档同样会体现新加入的请求参数和请求体：

![image-20240526214800262](C:\Users\often\AppData\Roaming\Typora\typora-user-images\image-20240526214800262.png)