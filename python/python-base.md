# 判断 content_type,接收Json参数
```python
    content_type = request.headers.get('Content-Type', '')

    if content_type == 'application/json':
        data = request.json
        if 'prompt' in data:
            search_text = data['prompt']
            return jsonify({'message': 'Text search request received', 'search_text': search_text})
    elif content_type.startswith('multipart/form-data'):
        if 'file' in request.files:
            file = request.files['file']
            if file and allowed_img(file.filename):
                save_disk(file)
                # TODO Img
                return jsonify({'msg': 'Image saved and search request received successfully'})
            elif file and allowed_file(file.filename):
                save_disk(file)
                # TODO File
                return jsonify({'msg': 'File saved and search request received successfully'})
            else:
                return jsonify({'error': 'Invalid file'})
        else: return jsonify({'error': 'No image provided'})
    else:
        return jsonify({'error': 'Unsupported Media Type'}), 415
```

# if 非null非空判断

你可以使用Python中的条件语句（`if`语句）来实现这个功能。以下是一个示例代码，演示如何检查请求 JSON 参数中的 `"context"` 是否存在且值为 `"LOAN"`：

```python
import json

# 假设这是你的请求 JSON 数据
request_data = '{"context": "LOAN", "other_key": "other_value"}'

# 解析 JSON 数据
data = json.loads(request_data)

# 检查 "context" 是否存在且值为 "LOAN"
if 'context' in data and data['context'] == 'LOAN':
    print("请求参数中的 'context' 不为空且为 'LOAN'")
else:
    print("请求参数中的 'context' 为空或不为 'LOAN'")
```

在这个示例中，首先使用 `json.loads()` 解析 JSON 字符串为 Python 字典。然后，通过 `if` 语句检查字典中是否包含键 `'context'`，并且其对应的值是否为 `'LOAN'`。根据条件的结果，输出相应的消息。



在Python中，可以使用条件语句来检查某个变量是否为空。一种常见的方法是使用布尔上下文，例如：

```python
# 定义一个变量
my_variable = "Hello"

# 使用if语句检查变量是否为空
if my_variable:
    print("变量不为空")
else:
    print("变量为空")
```

在这个示例中，如果 `my_variable` 变量不为空，则 `if` 语句将执行其下面的代码块，打印出 `"变量不为空"`；如果 `my_variable` 为空，则执行 `else` 语句块，打印出 `"变量为空"`。

值得注意的是，在Python中，空字符串 `""`、空列表 `[]`、空字典 `{}`、空集合 `set()`、空元组 `()`、空值 `None` 等都被视为假（False），因此在布尔上下文中会被视为空。



如果你想检查一个变量既不是 `None` 也不是空（例如空字符串、空列表等），你可以结合使用逻辑运算符 `and` 和相应的条件来实现。以下是一个示例：

```python
# 定义一个变量
my_variable = "Hello"

# 使用if语句检查变量是否不是None且不为空
if my_variable is not None and my_variable != "":
    print("变量既不是None也不为空")
else:
    print("变量是None或者为空")
```

在这个示例中，`if` 语句使用了两个条件：`my_variable is not None` 检查变量是否不是 `None`，以及 `my_variable != ""` 检查变量是否不是空字符串。只有当这两个条件都满足时，`if` 语句才会执行其下面的代码块，打印出 `"变量既不是None也不为空"`。
