# 项目级 Copilot 指令 - 代码生成技能

## 通用规则

1. **代码质量**：生成的所有代码必须包含适当的错误处理，不能忽略异常。
2. **安全性**：避免安全漏洞（如 SQL 注入、XSS、CSRF），对外部输入进行验证和清理。
3. **性能**：优先考虑性能，避免不必要的循环、重复计算或内存泄漏。
4. **可读性**：代码必须清晰、自文档化。必要时添加注释解释"为什么"而非"是什么"。
5. **测试**：关键函数应包含单元测试。

---

## Python 规则

### 风格规范
- 遵循 **PEP 8** 标准。
- 使用 **4 空格缩进**，不使用制表符。
- 行宽限制为 **88 字符**（兼容 Black formatter）。
- 使用类型注解（Type Hints）为所有函数参数和返回值声明类型。
- 使用 `snake_case` 命名变量和函数，`PascalCase` 命名类，`UPPER_CASE` 命名常量。

### 代码组织
- 导入顺序：标准库 → 第三方库 → 本地模块，每组之间空一行。
- 优先使用 `pathlib` 而非 `os.path` 处理路径。
- 使用 `f-string` 进行字符串格式化。

### 错误处理
- 使用特定异常类型，避免裸 `except:`。
- 使用 `contextlib.suppress()` 忽略预期内的异常。
- 对资源使用 `with` 语句（上下文管理器）。

### 示例

```python
from typing import Optional


def calculate_total_price(items: list[dict], discount_rate: float = 0.0) -> float:
    """计算商品总价，可选应用折扣。

    Args:
        items: 商品列表，每项包含 'name' 和 'price' 键。
        discount_rate: 折扣率，范围 0.0 ~ 1.0。

    Returns:
        应用折扣后的总价。

    Raises:
        ValueError: 当折扣率超出有效范围时。
    """
    if not 0.0 <= discount_rate <= 1.0:
        raise ValueError("折扣率必须在 0.0 到 1.0 之间")

    total = sum(item["price"] for item in items)
    return total * (1 - discount_rate)
```

---

## JavaScript / TypeScript 规则

### 风格规范
- 遵循 **ESLint 推荐规则** + **Prettier** 格式化。
- 使用 **2 空格缩进**，不使用制表符。
- 行宽限制为 **80 字符**。
- 使用 **`const`** 和 **`let`**，不使用 `var`。
- 使用 **`camelCase`** 命名变量和函数，`PascalCase` 命名类和组件，`UPPER_CASE` 命名常量。

### TypeScript 优先
- 新代码优先使用 TypeScript 而非 JavaScript。
- 为所有函数参数和返回值定义类型。
- 避免使用 `any`，优先使用 `unknown` 并在使用时进行类型收窄。
- 使用 `interface` 定义对象形状，使用 `type` 定义联合类型和工具类型。

### 异步处理
- 优先使用 `async/await` 而非 `.then()` 链。
- 所有异步操作必须包含适当的错误处理（try/catch）。
- 避免回调地狱。

### 示例

```typescript
/**
 * 获取用户信息并缓存结果。
 *
 * @param userId - 用户唯一标识符
 * @returns 用户信息对象，如果未找到则返回 null
 * @throws 当网络请求失败时抛出
 */
async function fetchUserInfo(userId: string): Promise<UserInfo | null> {
  try {
    const response = await fetch(`/api/users/${userId}`);
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }
    return await response.json() as UserInfo;
  } catch (error) {
    console.error(`获取用户 ${userId} 信息失败:`, error);
    throw error;
  }
}

interface UserInfo {
  id: string;
  name: string;
  email: string;
  createdAt: Date;
}
```

---

## 此项目的特定规则

### 项目结构
- HTML 文件遵循无障碍（Accessibility）最佳实践，使用语义化标签。
- 保持 `index.html` 和 `parten.html` 的风格一致性。
- CSS 使用现代布局（Flexbox/Grid）和 CSS 变量管理主题。

### 代码提交
- 提交信息使用中文或英文均可，但需要清晰描述变更内容。
- 保持代码简洁，避免过度工程化。
