# 算子大全 #

### 一、source ###

#### 1. fromCollection

1.1 map

```java
// 使用map将List转换为一个Scala的样例类

case class User(name: String, id: String)

val userDataSet: DataSet[User] = textDataSet.map {
  text =>
    val fieldArr = text.split(",")
    User(fieldArr(0), fieldArr(1))
}
userDataSet.print()
```

类似于 一麻袋的苹果，做完map操作后，变成，一麻袋的苹果皮

1.2

### 二、 transform



