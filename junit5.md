# Junit5

JUnit 5是由JUnit Platform + JUnit Jupiter + JUnit Vintage組合而成的測試框架。

* JUnit Platform -> 是在JVM平台上運行的測試引擎以及框架，可運作在常見的IDE中(Intellij IDEA、Eclipse、NetBeans、VS Code...)，以及各種打包工具(Gradle、Maven、Ant...)
* JUnit Jupiter -> JUint5的擴充API
* JUnit Vintage -> 可向下相容JUnit3、Junit4的平台

Spring boot在專案初始時就會自動掛載Junit5，因此不需要額外引入套件。
