# Python 基础学习总结（一）

> CS224N 前置学习 · Python Day 1  
> 学习目标：掌握后续 NumPy、PyTorch 和 NLP 编程常用的 Python 基础语法，并能独立完成简单的文本分词与词频统计。

## 一、变量与基本数据类型

Python 变量无需提前声明类型，可以用 `type()` 查看当前类型。

```python
print("Hello Python")

name = "Tom"        # str：字符串
age = 20            # int：整数
score = 3.14        # float：浮点数
is_student = True   # bool：布尔值

print(type(age))     # <class 'int'>
```

| 类型 | 含义 | 示例 |
| --- | --- | --- |
| `int` | 整数 | `10` |
| `float` | 浮点数 | `3.14` |
| `str` | 字符串 | `"hello"` |
| `bool` | 布尔值 | `True`、`False` |

**易错点：**`=` 用于赋值，`==` 用于判断两边是否相等。

## 二、条件判断：`if` / `elif` / `else`

```python
score = 85

if score >= 90:
    print("优秀")
elif score >= 60:
    print("及格")
else:
    print("不及格")
```

条件从上到下判断。某个分支成立后，其余分支不再执行。常用比较运算符有 `>`、`<`、`>=`、`<=`、`==` 和 `!=`。

**注意：**Python 使用缩进表示代码块，通常每层缩进 4 个空格。

## 三、循环：`for` / `while`

### `for` 与 `range()`

```python
for i in range(1, 6):
    print(i)  # 依次输出 1、2、3、4、5
```

`range(start, end, step)` 从 `start` 开始，到 `end` 之前结束；`step` 是步长。

```python
range(5)         # 0, 1, 2, 3, 4
range(1, 6)      # 1, 2, 3, 4, 5
range(0, 10, 2)  # 0, 2, 4, 6, 8
```

### `while`、`break` 与 `continue`

```python
i = 0
while i < 5:
    print(i)
    i += 1
```

- `break`：结束整个循环。
- `continue`：跳过本次循环的剩余代码，进入下一次循环。

使用 `while` 时，要确保循环条件最终会改变，避免死循环。

## 四、列表 `list` 与切片

列表保存一组有顺序的元素，下标从 `0` 开始，也可以用负数下标从末尾访问。

```python
words = ["I", "love", "NLP"]

print(words[0])       # I
print(words[-1])      # NLP
words[1] = "study"    # 修改元素
words.append("AI")    # 在末尾添加元素
print(len(words))     # 4
print("NLP" in words)  # True

for word in words:
    print(word)
```

切片格式为 `序列[start:end:step]`，包含 `start`，不包含 `end`。

```python
words[:3]    # 前 3 个元素
words[2:]    # 从下标 2 到末尾
words[1:4]   # 下标 1、2、3
words[::-1]  # 倒序后的新列表
```

`range()` 和切片都遵循“左闭右开”的结束位置规则。

## 五、字符串处理

`split()` 把字符串拆成列表；`join()` 把字符串列表连接成一个字符串。

```python
sentence = "I love natural language processing"
words = sentence.split()
print(words)  # ['I', 'love', 'natural', 'language', 'processing']

words = ["I", "love", "NLP"]
sentence = " ".join(words)
print(sentence)  # I love NLP
```

`lower()` 和 `upper()` 分别把字符串转换为小写和大写，并返回新字符串。

```python
text = "I Love NLP"
print(text.lower())  # i love nlp
print(text.upper())  # I LOVE NLP
```

简单的英文文本预处理可先用 `lower()` 统一大小写，再用 `split()` 得到 token 列表。

## 六、字典 `dict`

字典保存 `key → value` 的映射，适合记录一个单词及其出现次数。

```python
student = {"name": "Tom", "age": 20}

print(student["name"])  # Tom
student["major"] = "CS" # 新 key：添加
student["age"] = 21     # 已有 key：修改
```

| 方法 | 作用 |
| --- | --- |
| `my_dict.keys()` | 获取所有 key |
| `my_dict.values()` | 获取所有 value |
| `my_dict.items()` | 获取 `(key, value)` 对 |

```python
word_count = {"apple": 3, "banana": 2, "orange": 1}

for word, count in word_count.items():
    print(word, count)
```

`items()` 每次提供一组键值对，循环中的两个变量分别接收 key 和 value。

## 七、核心练习：词频统计

第一次遇到单词时将次数设为 `1`；再次遇到时将原有次数加 `1`。

```python
words = ["apple", "banana", "apple", "orange", "banana", "apple"]
word_count = {}

for word in words:
    if word in word_count:
        word_count[word] += 1
    else:
        word_count[word] = 1

print(word_count)
# {'apple': 3, 'banana': 2, 'orange': 1}
```

## 八、迷你 NLP 流程

```text
原始文本 → 转小写 → 按空格分词 → token 列表 → 词频字典
```

```python
sentence = "I Love NLP and I Love Python"
words = sentence.lower().split()
word_count = {}

for word in words:
    if word in word_count:
        word_count[word] += 1
    else:
        word_count[word] = 1

print(words)
# ['i', 'love', 'nlp', 'and', 'i', 'love', 'python']
print(word_count)
# {'i': 2, 'love': 2, 'nlp': 1, 'and': 1, 'python': 1}
```

这条流程是后续 `sentence → tokens → token ids → embeddings` 的起点。这里的 `split()` 仅适合演示以空格分隔的简单英文文本；实际 NLP 项目通常需要更合适的分词方法。

## 九、自测清单

- [ ] 能区分赋值 `=` 与比较 `==`。
- [ ] 能编写 `if` / `elif` / `else`，并正确缩进。
- [ ] 知道 `range()` 不包含结束值。
- [ ] 会使用 `for`、`while`、`break` 和 `continue`。
- [ ] 知道列表下标从 `0` 开始，会使用 `append()` 和 `len()`。
- [ ] 会使用切片 `[start:end:step]`。
- [ ] 会使用 `split()`、`join()`、`lower()` 和 `upper()`。
- [ ] 理解字典的 key 与 value，会用 `items()` 遍历。
- [ ] 能不看示例，独立写出词频统计程序。
- [ ] 能解释简单文本预处理的每一步。

**下一阶段：**学习 `tuple`、`set` 等数据结构，继续过渡到词表构建。参见 [Python 数据结构进阶学习总结（二）](Python_数据结构进阶_第二阶段.md)。

**依据：**[CS224N Python Day 1 知识清单](CS224N_Python_Day1_知识清单.pdf)。
