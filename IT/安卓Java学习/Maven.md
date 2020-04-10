### Maven

https://www.jianshu.com/p/b07e7dbd8e4c

#### 定义

对一个项目用对象描述方法来表示

1. 项目对象模型POM（project object model)

   一个对象有各种属性，类似的把一个项目也用这种方法来构成对象，其中属性有groupID、version、type，由于项目中Java会被继承，因此，项目对象会存在dependencies、parent和module等属性

   又XML文件具有很强的表达能力，一个Java对象可以用xml文件来描述，所以把项目对象用xml文件来表示时，即为pom.xml因此，pom.xml其实就是对PO对象的XML描述

#### 构建

项目的构建过程对应的是PO对象的build属性，对应pom.xml中也就是<build>元素中的内容

##### Lifecycle

生命周期，一个maven的构建过程对应一个生命周期，一个lifecycle分为多个阶段，叫做phase

标准的lifecycle有如下phase

``` 
validate： 用于验证项目的有效性和其项目所需要的内容是否具备
initialize：初始化操作，比如创建一些构建所需要的目录等。
generate-sources：用于生成一些源代码，这些源代码在compile phase中需要使用到
process-sources：对源代码进行一些操作，例如过滤一些源代码
generate-resources：生成资源文件（这些文件将被包含在最后的输入文件中）
process-resources：对资源文件进行处理
compile：对源代码进行编译
process-classes：对编译生成的文件进行处理
generate-test-sources：生成测试用的源代码
process-test-sources：对生成的测试源代码进行处理
generate-test-resources：生成测试用的资源文件
process-test-resources：对测试用的资源文件进行处理
test-compile：对测试用的源代码进行编译
process-test-classes：对测试源代码编译后的文件进行处理
test：进行单元测试
prepare-package：打包前置操作
package：打包
pre-integration-test：集成测试前置操作   
integration-test：集成测试
post-integration-test：集成测试后置操作
install：将打包产物安装到本地maven仓库
deploy：将打包产物安装到远程仓库
```

在maven中，你执行任何一个phase时，maven会将其之前的phase都执行.

#####  goal

每一个phase的具体动作有goal定义

一个goal在maven中就是于一个mojo（Maven old java object)，Mojo抽象类中定义了一个execute（）方法，goal中的具体动作就是在execute方法中实现

Mojo类存放在maven plugin

plugin就是一个maven项目，它会引用maven的一些API

在执行具体的构建时，我们需要为lifecycle的每个phase都绑定一个goal，这样才能够在每个步骤执行一些具体的动作。

##### 插件



