# FastAPI 入门教程

> 由@szw0407编写，献给@0nlineTek-Web-XLS的小伙伴们

在阅读这个教程之前，你需要系统了解一下Python和面向对象的程序设计理念，并且较为熟练地使用Python编写相对功能复杂的程序。

fastAPI是一个很新的东西，比起著名的Django或者flask。它的使用人数真的相对少，但是这个项目看起来前景很好，因为它性能较好，而且专注于API的部分，相对轻量级。

如果对于高并发有较高要求，或许使用Golang编写会更合理；而Python更适合做相对复杂的项目，比如在后端进行数据处理甚至科学计算，最后使用fastAPI与前端页面完成交互。这一点，除了Ruby有一部分专门针对数据分析、科学计算的设计，可能只有Python的庞大的生态能允许了。

> 人生苦短，我用Python。

## 安装

```bash
pip install fastapi

pip install "uvicorn[standard]"
```

安装 fastAPI 以及 ASGI 服务器（使用为高性能高并发优化的uvicorn）

当然也可以省事一些：

```powershell
pip install "fastapi[all]"
```

## 简单体验

将示例程序复制到 `main.py` 内：

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def root():
    return {"message": "Hello World"}
```

然后在终端中输入

```bash
uvicorn main:app --reload
```

打开浏览器访问`localhost:8000`即可看到这个hello world的信息。

打开`localhost:8000/docs`即可访问swagger文档——一个交互式OpenAPI文档。

打开`localhost:8000/redoc`即可访问ReDoc文档

现在我们初体验结束，开始分析这个示例程序：

- 从`fastapi`库导入了`FastAPI`类，我们使用`app = FastAPI()`创建了一个实例，这个实例就是我们的应用程序。
- 使用`@app.get("/")`装饰器，将`root()`函数绑定到了`/`路径上，这个函数理论上返回了一个字典，然后被转化为了响应json传输给客户端。
- 至于这个转换的过程是如何实现的，考虑`@app.get()`装饰器的作用，这个装饰器将`root()`函数转换成了一个`GET`请求的处理函数，而`root()`函数的返回值将会被转换成JSON格式的数据返回给客户端。

## 路径参数

在`fastAPI`中，路径参数是通过`{}`来定义的，比如`/items/{item_id}`，这个路径中的`item_id`就是一个路径参数。

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_item(item_id):
    return {"item_id": item_id}
```

当我们试图访问`/items/123`时，返回`{"item_id": 123}`。

但是这样的话，我们就无法对路径参数进行类型检查了，比如我们希望`item_id`是一个整数，那么我们可以这样写：

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_item(item_id: int):
    return {"item_id": item_id}
```

这样，如果我们访问`/items/123`，返回`{"item_id": 123}`，但是如果我们访问`/items/foo`，则会获得一个$422$的HTTP状态码，表示请求无效，因为`foo`不是一个整数。返回的信息如下：

```json
{
  "detail": [
    {
      "loc": [
        "path",
        "item_id"
      ],
      "msg": "value is not a valid integer",
      "type": "type_error.integer"
    }
  ]
}
```

## 查询参数

查询参数是通过`?`来定义的，比如`/items/?skip=0&limit=10`，这个路径中的`skip`和`limit`就是查询参数。

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/")
async def read_item(skip: int = 0, limit: int = 10):
    return {"skip": skip, "limit": limit}
```

我们可以看到，`skip`和`limit`都有默认值，这意味着它们是可选的，如果我们访问`/items/`，则返回`{"skip": 0, "limit": 10}`，如果我们访问`/items/?skip=10`，则返回`{"skip": 10, "limit": 10}`；而如果我们访问`/items/?skip=10&limit=20`，则返回`{"skip": 10, "limit": 20}`。

## 请求体

刚才我们讨论的都是GET请求，而GET请求的参数都是通过路径参数和查询参数来传递的，但是有时候我们需要通过请求体来传递参数，比如POST请求。当然，发送请求体的请求不仅仅只有POST，还有PUT、PATCH等。

> 为什么要发送请求体而不是使用路径参数和查询参数呢？
>
> 这是因为GET请求的参数都是在URL中，而URL的长度是有限制的，而且URL中的参数都是明文的，不适合传输敏感信息（比如密码）。试想当你翻看你的浏览器历史记录时，你会看到很多URL，而这些URL中可能包含了你的密码，这是非常危险的。更何况历史记录可能会被网站使用JavaScript读取，这样你的密码就泄露了。而请求体中的参数是不会出现在URL中的，所以更加安全。

由于请求体通常是JSON格式的，所以我们需要使用`Pydantic`库来处理JSON数据。

```python
# for Python 3.10+
from fastapi import FastAPI
from pydantic import BaseModel


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


app = FastAPI()


@app.post("/items/")
async def create_item(item: Item):
    return item
```

这里我们定义了一个`Item`类，它继承自`BaseModel`，并且定义了`name`、`description`、`price`和`tax`四个属性，其中`name`和`price`是必须的，而`description`和`tax`是可选的。

