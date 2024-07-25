在 spaCy 中进行词性还原（Lemmatization）非常简单。下面是一个基本示例，展示了如何使用 spaCy 对文本进行词性还原：

1. 安装 spaCy 和所需的语言模型（如果还没有安装）：

```bash
pip install spacy
python -m spacy download en_core_web_sm
```

2. 使用 spaCy 进行词性还原：

```python
import spacy

# 加载语言模型
nlp = spacy.load("en_core_web_sm")

# 处理文本
text = "The striped bats are hanging on their feet for best."
doc = nlp(text)

# 进行词性还原并输出结果
for token in doc:
    print(f"Token: {token.text}, Lemma: {token.lemma_}")
```

在上面的代码中，我们首先加载 spaCy 的语言模型（`en_core_web_sm`），然后使用 `nlp` 处理文本。处理后的 `doc` 对象包含了文本中的所有单词（tokens）。通过遍历 `doc` 对象，我们可以访问每个 token 的原始形式和词性还原形式（通过 `token.lemma_`）。

以下是上述代码的输出示例：

```
Token: The, Lemma: the
Token: striped, Lemma: stripe
Token: bats, Lemma: bat
Token: are, Lemma: be
Token: hanging, Lemma: hang
Token: on, Lemma: on
Token: their, Lemma: their
Token: feet, Lemma: foot
Token: for, Lemma: for
Token: best, Lemma: good
Token: ., Lemma: .
```

在这个示例中，"striped" 被还原为 "stripe"，"bats" 被还原为 "bat"，"are" 被还原为 "be"，依此类推。

通过这种方式，你可以很容易地使用 spaCy 对文本进行词性还原。

在自然语言处理（NLP）中，stop words 是指那些在分析和处理文本时被认为没有重要意义的常见词。这些词通常在文本中频繁出现，但对理解文本的实际内容贡献不大。常见的 stop words 包括 "the", "is", "in", "and" 等。

### spaCy 的 Stop Words

spaCy 提供了一个内置的 stop words 列表。你可以访问、查看和修改这个列表。

#### 查看 spaCy 的 Stop Words 列表

你可以使用以下代码查看 spaCy 的 stop words 列表：

```python
import spacy

# 加载语言模型
nlp = spacy.load("en_core_web_sm")

# 获取 stop words 列表
stop_words = nlp.Defaults.stop_words

# 输出 stop words 列表
print(stop_words)
```

#### 检查某个词是否是 Stop Word

你可以使用 `token.is_stop` 属性检查某个词是否是 stop word：

```python
import spacy

# 加载语言模型
nlp = spacy.load("en_core_web_sm")

# 处理文本
doc = nlp("This is a sample sentence.")

# 输出每个词及其是否为 stop word
for token in doc:
    print(f"Token: {token.text}, is_stop: {token.is_stop}")
```

#### 添加和移除 Stop Words

你可以动态地添加或移除 stop words：

**添加 stop word**

```python
import spacy

# 加载语言模型
nlp = spacy.load("en_core_web_sm")

# 添加新的 stop word
nlp.Defaults.stop_words.add("sample")
nlp.vocab["sample"].is_stop = True

# 检查 "sample" 是否为 stop word
print(nlp.vocab["sample"].is_stop)  # 输出 True
```

**移除 stop word**

```python
import spacy

# 加载语言模型
nlp = spacy.load("en_core_web_sm")

# 移除 stop word
nlp.Defaults.stop_words.remove("the")
nlp.vocab["the"].is_stop = False

# 检查 "the" 是否为 stop word
print(nlp.vocab["the"].is_stop)  # 输出 False
```

通过这些方法，你可以根据需要自定义 spaCy 的 stop words 列表，从而更好地适应你的具体应用场景。