# 第9章：测试策略

> 质量保证从测试开始。

## 9.1 测试的重要性

### 为什么需要测试
- 确保功能正确性
- 防止回归问题
- 提高代码质量
- 简化维护成本

### AI辅助测试的优势
- 快速生成测试用例
- 覆盖边界条件
- 生成mock数据
- 识别测试盲点

## 9.2 测试类型

### 单元测试
测试单个函数或方法。

**提示词模板**：
```
请为以下函数编写单元测试：
[函数代码]

功能描述：[说明]
输入参数：[列举]
返回值：[描述]

需要测试：
1. 正常情况
2. 边界条件
3. 异常处理
4. 边界值

使用pytest框架，包含fixture和mock。
```

### 集成测试
测试多个组件协作。

**提示词模板**：
```
请为以下API端点编写集成测试：
[API代码]

测试场景：
1. 正常请求流程
2. 错误响应处理
3. 数据库交互
4. 外部API调用

使用pytest和requests库。
```

### 端到端测试
测试完整用户流程。

**提示词模板**：
```
请为以下用户流程编写端到端测试：

流程描述：
1. 用户打开页面
2. 输入查询内容
3. 点击生成按钮
4. 等待结果
5. 下载音频

使用Selenium或Playwright。
```

## 9.3 测试框架

### Pytest（Python）

**配置文件**：pytest.ini
```ini
[pytest]
testpaths = tests
python_files = test_*.py
python_classes = Test*
python_functions = test_*
addopts =
    -v
    --strict-markers
    --tb=short
```

**Fixtures**：conftest.py
```python
@pytest.fixture
def client():
    app = create_app(testing=True)
    with app.test_client() as c:
        yield c
```

### JUnit（Java）

### Jest（JavaScript）

## 9.4 测试设计模式

### AAA模式
- Arrange（准备）：设置测试数据
- Act（执行）：运行被测代码
- Assert（断言）：验证结果

### Given-When-Then
```gherkin
Given 用户已登录
When 用户点击生成按钮
Then 应该显示成功消息
```

## 9.5 AI辅助生成测试

### 生成测试数据

**提示词**：
```
请为以下函数生成测试用例的数据：

函数签名：def calculate_price(quantity, unit_price, discount=0)

要求：
- 生成10组正常数据
- 生成5组边界数据
- 生成5组异常数据
- 包含预期输出

使用表格形式展示。
```

### 生成Mock对象

**提示词**：
```
请为以下类创建Mock对象用于测试：

[类定义]

需要mock的方法：
1. [方法1]：返回特定值
2. [方法2]：抛出异常
3. [方法3]：调用次数验证

使用unittest.mock和pytest。
```

### 生成边界测试

**提示词**：
```
函数接收一个整数数组，请帮我生成全面的边界测试：

测试场景：
- 空数组
- 单元素数组
- 已排序数组
- 逆序数组
- 重复元素数组
- 大量数据数组

提供测试代码和输入数据。
```

## 9.6 测试覆盖率

### 覆盖率工具
- pytest-cov：Python
- coverage.py：Python
- Jest：JavaScript

**提示词**：
```
请帮我分析以下代码的测试覆盖率：

[代码]

当前测试：
[测试代码]

请指出：
1. 哪些分支未覆盖
2. 哪些边界条件未测试
3. 建议补充的测试用例
```

### 覆盖率目标
- 语句覆盖率：>80%
- 分支覆盖率：>70%
- 函数覆盖率：>90%

## 9.7 测试最佳实践

### 1. 测试命名
```python
def test_user_login_with_valid_credentials():
    pass

def test_user_login_with_invalid_credentials_raises_error():
    pass
```

### 2. 测试独立性
- 每个测试独立运行
- 不依赖执行顺序
- 清理副作用

### 3. 测试可读性
```python
def test_should_return_error_when_quantity_is_negative():
    # Given
    quantity = -1
    unit_price = 100

    # When
    with pytest.raises(ValueError):
        calculate_price(quantity, unit_price)

    # Then
    # Exception raised as expected
```

### 4. 测试数据管理
- 使用fixtures
- 参数化测试
- 测试数据工厂

## 9.8 持续集成测试

### CI配置
```yaml
# .github/workflows/test.yml
name: Tests
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: '3.9'
      - name: Install dependencies
        run: |
          pip install -r requirements.txt
          pip install pytest pytest-cov
      - name: Run tests
        run: pytest --cov=src --cov-report=xml
      - name: Upload coverage
        uses: codecov/codecov-action@v2
```

## 9.9 测试驱动开发（TDD）

### TDD流程
1. **Red**：写一个失败的测试
2. **Green**：写代码让测试通过
3. **Refactor**：重构代码

### AI辅助TDD
```
我想要实现[功能描述]。

请帮我：
1. 先编写测试用例（遵循TDD）
2. 然后实现功能代码
3. 确保测试通过

使用pytest框架。
```

## 9.10 实战案例

### 案例1：播客应用测试

**测试结构**：
```
tests/
├── conftest.py              # 共享fixtures
├── test_llm/               # LLM测试
├── test_image/             # 图像测试
├── test_routes/            # 路由测试
└── test_tasks/             # 任务测试
```

**AI生成测试**：
```python
# AI生成的测试示例
@pytest.mark.llm
def test_podcast_generation_with_valid_input():
    # Given
    query = "AI技术发展趋势"
    duration = 5
    speakers = ["CN-Man-Beijing-V2"]

    # When
    response = client.post('/api/generate', json={
        'query': query,
        'duration': duration,
        'speakers': speakers
    })

    # Then
    assert response.status_code == 200
    data = response.get_json()
    assert data['success'] == True
    assert 'task_id' in data
```

## 9.11 测试提示词模板

### 功能测试模板
```
请为[功能名称]编写测试用例。

功能描述：[详细说明]
输入参数：
- param1: [说明]
- param2: [说明]

测试场景：
1. 场景1
2. 场景2
3. 边界场景

使用[测试框架]，包含完整的断言。
```

### 异常测试模板
```
请为以下异常情况编写测试：

[代码片段]

可能抛出：
1. ValueError
2. TypeError
3. KeyError

请验证：
1. 正确抛出异常
2. 异常信息准确
3. 正常情况不受影响
```

### 性能测试模板
```
请编写性能测试：

[代码描述]

测试指标：
- 响应时间 < 1s
- 内存占用 < 100MB
- 并发支持 > 10 req/s

使用locust或类似工具。
```

## 9.12 小结

AI辅助测试的价值：

1. **快速生成**：快速创建测试用例
2. **全面覆盖**：识别测试盲点
3. **持续改进**：根据代码变化更新测试

测试质量保证：

1. **编写测试**：AI辅助生成
2. **审查测试**：人工检查覆盖
3. **运行测试**：CI/CD自动化
4. **维护测试**：随代码更新

**下一章**：我们将探讨文档与知识管理。
