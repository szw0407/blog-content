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


## 基础使用

### 路径参数

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

这样，如果我们访问`/items/123`，返回`{"item_id": 123}`，但是如果我们访问`/items/foo`，则会获得一个 422 的HTTP状态码，表示请求无效，因为`foo`不是一个整数。返回的信息如下：

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

### 查询参数

查询参数是通过`?`来定义的，比如`/items/?skip=0&limit=10`，这个路径中的`skip`和`limit`就是查询参数。

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/")
async def read_item(skip: int = 0, limit: int = 10):
    return {"skip": skip, "limit": limit}
```

我们可以看到，`skip`和`limit`都有默认值，这意味着它们是可选的，如果我们访问`/items/`，则返回`{"skip": 0, "limit": 10}`，如果我们访问`/items/?skip=10`，则返回`{"skip": 10, "limit": 10}`；而如果我们访问`/items/?skip=10&limit=20`，则返回`{"skip": 10, "limit": 20}`。

### 请求体

刚才我们讨论的都是GET请求，而GET请求的参数都是通过路径参数和查询参数来传递的，但是有时候需要将数据从客户端（例如浏览器）发送给 API ，我们将其作为 **请求体** 发送。我们需要通过请求体来传递参数，比如POST请求。当然，发送请求体的请求不仅仅只有POST，还有PUT、PATCH等。

> #### 为什么要发送请求体而不是使用路径参数和查询参数呢？
>
> 这是因为GET请求的参数都是在URL中，而URL的长度是有限制的，而且URL中的参数都是明文的，不适合传输敏感信息（比如密码）。试想当你翻看你的浏览器历史记录时，你会看到很多URL，而这些URL中可能包含了你的密码，这是非常危险的。更何况历史记录可能会被网站使用JavaScript读取，这样你的密码就泄露了。而请求体中的参数是不会出现在URL中的，所以更加安全。


**请求体**是客户端发送给 API 的数据。**响应体**是 API 发送给客户端的数据。

你的 API 几乎总是要发送响应体。但是客户端并不总是需要发送请求体。

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

`BaseModel`是`pydantic`库中的一个类，它的作用是将我们定义的类转换成一个模型，这个模型可以用来进行数据校验和数据转换。pydantic库还有一个作用是将模型转换成JSON格式的数据。

`@app.post()`装饰器将`create_item()`函数转换成了一个POST请求的处理函数，并将请求体中的JSON数据转换成了`Item`类的实例，这个实例就是我们的模型，我们可以对它进行数据校验和数据转换，最后返回这个模型，这个模型会被转换成JSON格式的数据返回给客户端。

如果我们访问`/items/`，并且发送一个JSON数据，比如`{"name": "Foo", "price": 42}`，则返回`{"name": "Foo", "description": null, "price": 42.0, "tax": null}`；

同样，如果我们访问`/items/`，并且发送一个JSON数据，比如`{"name": "Foo", "price": "bar"}`，这个请求体校验不通过，会获得一个 422 的HTTP状态码，表示请求无效，因为`price`不是一个浮点数。返回`{"detail":[{"loc":["body","price"],"msg":"value is not a valid float","type":"type_error.float"}]}`。

值得注意的是，在Json中，`null`表示空值，而在Python中，`None`表示空值。二者之间还有一些其他的差异，比如`true`和`false`在Python中分别表示`True`和`False`，`"foo"`和`'foo'`在Python中都表示`"foo"`，但是在JSON中，`"foo"`是合法的，而`'foo'`是不合法的，等等。

接下来我们来看一个更加复杂的例子：

```python
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
    item_dict = item.dict()
    if item.tax:
        price_with_tax = item.price + item.tax
        item_dict.update({"price_with_tax": price_with_tax})
    return item_dict
```

这里我们在`create_item()`函数中，对`Item`类的实例进行了一些处理，比如如果`tax`不为空，则计算`price_with_tax`，然后将`price_with_tax`添加到`item_dict`中，最后返回`item_dict`。

这里，我们使用了pydantic的特性，直接使用`.`来访问模型的属性，这样我们就可以像操作字典一样操作模型了。

> #### 为什么我们需要使用Pydantic而不是直接将类型规定为`dict`呢？
>
> 我们查看一下Swagger文档，所定义模型的 JSON 模式将成为生成的 OpenAPI 模式的一部分，并且在交互式 API 文档中展示：
> ![swagger-example](https://fastapi.tiangolo.com/img/tutorial/body/image01.png)
> 而且还将在每一个需要它们的路径操作的 API 文档中使用
> ![swagger-example](https://fastapi.tiangolo.com/img/tutorial/body/image02.png)
> 这样，我们就可以在Swagger文档中看到我们定义的模型了，而不是看到一个`dict`（显示为一个简单的`{}`）。
>
> 同时，我们还能在编辑器中，你会在函数内部的任意地方得到类型提示和代码补全——如果你接收的是一个 dict 而不是 Pydantic 模型，则不会发生这种情况；
> ![vscode-example](https://fastapi.tiangolo.com/img/tutorial/body/image03.png)
> 还会获得对不正确的类型操作的错误检查：
> ![vscode-example](https://fastapi.tiangolo.com/img/tutorial/body/image04.png)
> 不论是VS code 还是 PyCharm，都会获得这些功能:
> ![pycharm-example](https://fastapi.tiangolo.com/img/tutorial/body/image05.png)
> 这并非偶然，整个框架都是围绕该设计而构建。并且在进行任何实现之前，已经在设计阶段经过了全面测试，以确保它可以在所有的编辑器中生效。Pydantic 本身甚至也进行了一些更改以支持此功能。

好了，现在我们已经可以通过这些方法，完成非常基本的API操作了。可以考虑开始写一个简单的API了。

由于我们尚未学习数据库的使用，如果需要往本地存储数据可以暂且使用`json`库，将数据存储在json文件中。这也是我们再很多简单的项目中使用的方法。

### 高级的校验

有的时候我们需要对于参数进行比较复杂的校验，比如我们不仅要求某个查询参数是一个字符串，还要求它的长度在某个范围内，这时候我们就需要使用`Query`函数了。

```python
from typing import Union

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Union[str, None] = Query(default=None, max_length=50)):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```

这里我们使用`Query`函数对`q`进行了校验，`Query`函数的第一个参数是默认值，第二个参数是最大长度（50），这样我们就可以对`q`进行校验了。

然后我们可以再增加一些校验，比如我们要求`q`的长度至少为3，那么我们可以这样写：

```python
from typing import Union

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Union[str, None] = Query(None, min_length=3, max_length=50)):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```

这样，如果我们访问`/items/?q=ab`，则会获得一个 422 的HTTP状态码，表示请求无效，因为`q`的长度不足3。返回`{"detail":[{"loc":["query","q"],"msg":"ensure this value has at least 3 characters","type":"value_error.any_str.min_length","ctx":{"limit_value":3}}]}`。

对于更加高级的校验，比如字符串要满足某个格式，我们会使用“正则表达式”。

```python
from typing import Union

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(
    q: Union[str, None] = Query(
        default=None, min_length=3, max_length=50, pattern="^fixedquery$"
    )
):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```

这个指定的正则表达式的意思是，字符串必须是`fixedquery`，否则就会校验失败。

如果你对所有的这些**正则表达式**概念感到迷茫，请不要担心，这不是必须掌握的内容，可以等需要使用的时候再说。对于许多人来说这都是一个困难的主题。你仍然可以在无需正则表达式的情况下做很多事情。但是，一旦你需要用到并去学习它们时，请了解你已经可以在 FastAPI 中直接使用它们。

需要注意的是，当我们使用`Query`的时候，没有`default`字段，这个值就是必须的。此外，有另一种方法可以显式的声明一个值是必需的，即将默认参数的默认值设为 `...`。对，你没看错，就是三个点构成[省略号](https://docs.python.org/3/library/constants.html#Ellipsis)！就像这个例子：

```python
from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: str = Query(default=..., min_length=3)):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```

此外，使用`Query`还可以声明它去接收一组值，或换句话来说，接收多个值：

```python
from typing import List, Union

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Union[List[str], None] = Query(default=None)):
    query_items = {"q": q}
    return query_items
```

这个时候，我们访问`/items/?q=foo&q=bar`，则返回`{"q": ["foo", "bar"]}`。可以观察一下Swagger文档，我们可以看到`q`的类型是可以填多个值的。

Query还有别的参数，就不多介绍了，可以去查文档~

