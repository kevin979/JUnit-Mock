---
description: 硬斷言
---

# Hard Assertions

```java
@Test
void test() {
    BigDecimal result = add(1, 2, 3, "4", "5", 6L);
    Assertions.assertEquals(new BigDecimal(21), result);
}

BigDecimal add(Object... objects) {
    BigDecimal result = BigDecimal.ZERO;
    for (Object o : objects) {
        result = result.add(new BigDecimal(o.toString()));
    }
    return result;
}
```

以上是硬斷言的測試範例，當測試通過時並不會產生多餘的訊息，只有測試未通過時會有錯誤訊息。

```java
@Test
void test() {
    BigDecimal result = add(1, 2, 3, "4", "5", 6L);
    Assertions.assertEquals(new BigDecimal(2), result);
}

BigDecimal add(Object... objects) {
    BigDecimal result = BigDecimal.ZERO;
    for (Object o : objects) {
        result = result.add(new BigDecimal(o.toString()));
    }
    return result;
}
```

```
// 執行結果
expected: <2> but was: <21>
Expected :2
Actual   :21
```

預期結果是2，但實際結果是21，所以此次測試未通過，未通過的測試會將訊息印出，讓開發人員可以核對與確認最終結果，以下是常用的斷言函數(不另寫範例，因執行後效果同上述範例)：

* assertFalse -> 判斷是否為false
* assertTrue -> 判斷是否為true
* assertEquals -> 判斷是否相同
* assertNotEquals -> 判斷是否不同
* assertNull -> 判斷是否為null
* assertNotNull -> 判斷是否不為null

除了以上這些斷言外，還有一種是常用且特別的測試方式：擷取Exception，在邏輯測試中常會有自定義的Exception，因此測試時也會針對這些Exception進行情境測試，確保Exception有正確的被拋出，以下範例示範如何截取Exception，並利用斷言判斷是否為正確的Exception。

```java
@Test
void test() {
    Executable executable = () -> throwException();
    Exceptions exceptions = Assertions.assertThrows(Exceptions.class, executable);
    Assertions.assertEquals(999, exceptions.getCode());
}

void throwException() {
    Exceptions exceptions = new Exceptions("TEST");
    exceptions.setCode(999);
    throw exceptions;
}

class Exceptions extends RuntimeException {
    private Integer code;
    private String message;
	
    Exceptions(String message) {
        super(message);
    }
	
    public Integer getCode() {
        return code;
    }
	
    public void setCode(Integer code) {
        this.code = code;
    }
	
    @Override
    public String getMessage() {
        return message;
    }
	
    public void setMessage(String message) {
        this.message = message;
    }
}
```

利用Executable儲存邏輯結果，再利用Assertions.assertThrows將Executable取出並定義類別，最後使用Assertions.assertEquals判斷是否為所需的Exception。
