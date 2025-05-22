## 💻 代码示例

### 基础对话

```python
import os
from openai import OpenAI
from dotenv import load_dotenv

# 加载环境变量
load_dotenv()

client = OpenAI(
    base_url=os.getenv("CLAUDE_BASE_URL"),
    api_key=os.getenv("CLAUDE_API_KEY")
)

def basic_chat():
    response = client.chat.completions.create(
        model="claude-sonnet-4-20250514",  # 或 "claude-opus-4-20250514"
        messages=[
            {"role": "user", "content": "你好，请介绍一下你自己"}
        ],
        max_tokens=1000,
        temperature=0.7
    )
    
    print("Claude 4 回复：")
    print(response.choices[0].message.content)

if __name__ == "__main__":
    basic_chat()
```

### 图像识别

```python
import os
from openai import OpenAI
from dotenv import load_dotenv

load_dotenv()

client = OpenAI(
    base_url=os.getenv("CLAUDE_BASE_URL"),
    api_key=os.getenv("CLAUDE_API_KEY")
)

def image_analysis():
    response = client.chat.completions.create(
        model="claude-sonnet-4-20250514",
        messages=[
            {
                "role": "user",
                "content": [
                    {"type": "text", "text": "请详细描述这张图片中的内容"},
                    {
                        "type": "image_url",
                        "image_url": {
                            "url": "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Gfp-wisconsin-madison-the-nature-boardwalk.jpg/2560px-Gfp-wisconsin-madison-the-nature-boardwalk.jpg"
                        }
                    }
                ]
            }
        ],
        max_tokens=500
    )
    
    print("图像分析结果：")
    print(response.choices[0].message.content)

if __name__ == "__main__":
    image_analysis()
```

### 代码生成与分析

```python
import os
from openai import OpenAI
from dotenv import load_dotenv

load_dotenv()

client = OpenAI(
    base_url=os.getenv("CLAUDE_BASE_URL"),
    api_key=os.getenv("CLAUDE_API_KEY")
)

def code_generation():
    response = client.chat.completions.create(
        model="claude-opus-4-20250514",  # 使用 Opus 4 获得最佳编码能力
        messages=[
            {
                "role": "user", 
                "content": """
                请帮我用Python写一个能处理嵌套JSON数据并提取特定字段的脚本，
                要求：
                1. 支持深层嵌套结构
                2. 可以通过路径表达式提取字段（如 'user.profile.name'）
                3. 处理缺失字段的情况
                4. 提供详细的代码注释和使用示例
                """
            }
        ],
        max_tokens=2000,
        temperature=0.3  # 降低温度以获得更准确的代码
    )
    
    print("生成的代码：")
    print(response.choices[0].message.content)

if __name__ == "__main__":
    code_generation()
```

### 流式响应

```python
import os
from openai import OpenAI
from dotenv import load_dotenv

load_dotenv()

client = OpenAI(
    base_url=os.getenv("CLAUDE_BASE_URL"),
    api_key=os.getenv("CLAUDE_API_KEY")
)

def streaming_chat():
    stream = client.chat.completions.create(
        model="claude-sonnet-4-20250514",
        messages=[
            {"role": "user", "content": "请写一篇关于人工智能发展的短文"}
        ],
        max_tokens=1000,
        stream=True
    )
    
    print("Claude 4 实时回复：")
    for chunk in stream:
        if chunk.choices[0].delta.content is not None:
            print(chunk.choices[0].delta.content, end="")
    print()

if __name__ == "__main__":
    streaming_chat()
```
