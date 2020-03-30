# JUnit5
## 注解
- @**Test**：被注解的方法是一个测试方法。与 JUnit 4 相同
- @**BeforeAll**：被注解的（静态）方法将在当前类中的所有 @Test 方法前执行一次
- @**BeforeEach**：被注解的方法将在当前类中的每个 @Test 方法前执行
- @**AfterEach**：被注解的方法将在当前类中的每个 @Test 方法后执行
- @**AfterAll**：被注解的（静态）方法将在当前类中的所有 @Test 方法后执行一次
- @**Disabled**：被注解的方法不会执行（将被跳过），但会报告为已执行
## 断言
> 标准断言
- **assertEquals**(expected, actual)：如果 expected 不等于 actual，则断言失败。
- **assertFalse**(booleanExpression)：如果 booleanExpression 不是 false，则断言失败。
- **assertNull**(actual)：如果 actual 不是 null，则断言失败。
- **assertNotNull**(actual)：如果 actual 是 null，则断言失败。
- **assertTrue**(booleanExpression)：如果 booleanExpression 不是 true，则断言失败。
> 新断言
- **asserAll**()：内部包含的所有断言都会执行，即使一个或多个断言失败也是如此。
- **assertThrows**()：抛出异常
## 前置条件
前置条件 (Assumption) 与断言类似，但前置条件必须为 true，否则测试将中止。与此相反，当断言失败时，则将测试视为已失败。测试方法只应在某些条件——前置条件下执行时，前置条件很有用。
