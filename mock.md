# Mock

Mockito是一個Java平台的開源測試框架，允許在自動化單元測試中創建測試替身對象（模擬對象）。

當使用 Mockito 在 mock 對象時，有一些限制需要遵守

* 不能 mock 靜態方法
* 不能 mock private 方法
* 不能 mock final class

因此在寫 code 時，需要做良好的功能拆分，才能夠使用 Mockito 的 mock 技術。

首先先建立一支class，裡面放一支未實作的method

```java
@Component
public class SystemUtils {

    public Boolean isSystemMaintain(String productID) {
        return null;
    }
}
```

再來準備一支class，裡面放一支會使用到SystemUtils中未實作的isSystemMaintain，以此模擬實際專案開發時狀況

```java
@Service
public class SystemService {
    @Autowired
    private SystemUtils systemUtils;

    public void run(String productID) {
        if (systemUtils.isSystemMaintain(productID)) {
            System.out.println("系統維護中");
        } else {
            System.out.println("系統運作中");
        }
    }
}
```

最後準備測試程式

```java
@Autowired
private SystemService systemService;

@MockBean
private SystemUtils systemUtils;

@Test
void test1() {
    Mockito.when(systemUtils.isSystemMaintain(Mockito.anyString())).thenReturn(false);
    systemService.run("123");
}

@Test
void test2() {
    Mockito.when(systemUtils.isSystemMaintain(Mockito.anyString())).thenReturn(true);
    systemService.run("123");
}
```

```
// 執行結果
系統運作中
系統維護中
```

從結果中可以看出Mock成功的偽造了回傳結果，因為SystemUtils的isSystemMaintain預設是回傳null的，該Method並沒有實作邏輯，所以不可能讓SystemService中的run正常運作。

如果是使用Spring boot 且須偽裝的類別是由Spring 利用IoC建立實例的話，那就需要使用註解 @MockBean 協助偽裝實例，那如果實例必須由程式中自行建立，則是使用Mockito.mock這方法。

```java
SystemUtils systemUtils = Mockito.mock(SystemUtils.class);
```

使用Mockito建立後的類別，其Method全部都會變成未實作狀態，其效果如下

```java
@Component
public class SystemUtils {

    public Boolean isSystemMaintain(String productID) {
        return null;
    }

    public LocalDateTime getSystemDateTime() {
        return LocalDateTime.now();
    }
}
```

先在原先的SystemUtils新增getSystemDateTime這個Method並且實作回傳當前時間，倘若將SystemUtils偽裝後不進行任何處理，則其回傳值效果是

```java
@MockBean
private SystemUtils systemUtils;

@Test
void test() {
    System.out.println(systemUtils.getSystemDateTime());
}
```

```
// 執行結果
null
```

因此若需要讓其回傳正確的值，則需呼叫thenCallRealMethod

```java
@MockBean
private SystemUtils systemUtils;

@Test
void test() {
    Mockito.when(systemUtils.getSystemDateTime()).thenCallRealMethod();
    System.out.println(systemUtils.getSystemDateTime());
}
```

```
// 執行結果
2024-01-31T16:34:58.828
```

所以將類別進行偽裝後，建議可在測試前將每個Method都呼叫thenCallRealMethod後，之後在單獨的測試方法中模擬結果。

```java
@Autowired
private SystemService systemService;

@MockBean
private SystemUtils systemUtils;

@BeforeEach
void before() {
    Mockito.when(systemUtils.getSystemDateTime()).thenCallRealMethod();
    Mockito.when(systemUtils.isSystemMaintain(Mockito.anyString())).thenCallRealMethod();
}

@Test
void test1() {
    Mockito.when(systemUtils.isSystemMaintain(Mockito.anyString())).thenReturn(false);
    systemService.run("123");
    System.out.println(systemUtils.getSystemDateTime());
}

@Test
void test2() {
    Mockito.when(systemUtils.isSystemMaintain(Mockito.anyString())).thenReturn(true);
    systemService.run("123");
    System.out.println(systemUtils.getSystemDateTime());
}
```

```
// 測試結果
系統運作中
2024-01-31T16:37:53.283
系統維護中
2024-01-31T16:37:53.306
```

除了thenReturn、thenCallRealMethod之外，還有一個thenThrow，顧名思義就是回傳Exception，其效過就不進行說明，與上述皆大同小異。

以下簡單範例示範斷言與模擬如何搭配使用

```java
@MockBean
private SystemUtils systemUtils;

@BeforeEach
void before() {
    Mockito.when(systemUtils.isSystemMaintain(Mockito.anyString())).thenCallRealMethod();
}

@Test
void test() {
    Mockito.when(systemUtils.isSystemMaintain(Mockito.anyString())).thenReturn(false);
    boolean isMaintain = systemUtils.isSystemMaintain("123");
    Assertions.assertEquals(false, isMaintain);
}
```
