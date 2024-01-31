---
description: 軟斷言
---

# Soft Assertions

一般來說測試原則是 「一個測試方法僅測試一種情境」，但有些邏輯會有多狀態需要進行判斷，如果使用硬斷言操作，可能會造成測試流程與時間過於冗長，因為遇到測試未通過就必須修復邏輯，一來一回會耗費很多時間，軟斷言就是為了此種情況而生，將所有測試結果蒐集完後再一次顯示，讓開發人員可以針對未通過的測試進行邏輯調整。

```java
@Test
void test() {
    BigDecimal result = add(1, 2, 3, "4", "5", 6L);
    Assertions.assertAll(
            () -> Assertions.assertEquals(new BigDecimal(21), result),
            () -> Assertions.assertTrue(new BigDecimal(50).compareTo(result) < 0),
            () -> Assertions.assertTrue(new BigDecimal(100).compareTo(result) > 0),
            () -> Assertions.assertNull(result)
    );
}
BigDecimal add(Object... objects) {
    BigDecimal result = BigDecimal.ZERO;
    for (Object o : objects) {
        result = result.add(new BigDecimal(o.toString()));
    }
    return result;
}
```

範例中可以看到有四種測試，1.預期結果為21、2.預期結果大於50、3.預期結果小於100、4.預期結果為null，這四個測試其中1、3可通過測試，2、4無法通過測試，因此執行後結果如下

```
//測試結果
Multiple Failures (2 failures)
	org.opentest4j.AssertionFailedError: expected: <true> but was: <false>
	org.opentest4j.AssertionFailedError: expected: <null> but was: <21>
Expected :null
Actual   :21
```

一般來說還是不建議使用軟斷言，原因是測試Method的命名是 「測試邏輯名稱\_測試預期結果」，如果採用軟斷言表示其測試結果會有多種，如此一來會造成Method的名稱過長不易閱讀，也不易管理(不確定那些測試未訂)。
