---
stage: dev
---

# 设计新库

本篇文章介绍设计新库的一般原则。Python、Django、Flutter等语言框架的具体实践详见各个语言和框架手册。

在TDD的模式下，我们需要先抽象真实场景设计用例，然后再根据目标设计具体实现。

## 设计用例

% 这一段表达的还比较混乱和模糊，文字还需要斟酌。

用例从真实场景中抽象和简化得到，开发过程中给库的实现提供目标，测试过程中作为验证标准。以`drf-versioned-models`为例，它的设计初衷是为了抽象数据模型的版本管理，因此用例抽象数据版本管理的常见场景。

设计用例项目。通常是`example`或`examples`文件夹。用例项目通常来自于真实项目的简化。
因此用例项目实现了课程数据模型和商品数据模型的简化版的模型，留下了部分和这个库相关的关键字段，着重突出其版本管理的思路。使用**更真实的**数据模型是为了帮助用例的读者建立更直观的感知。

设计单元测试用例。通常是`test`或者`tests`文件夹。单元测试项目则更重视贴近库本身对概念的定义，用例往往使用**更抽象的**、更贴合库的概念，以提高单元测试可读性，也提高库的普适性。
比如，`tests`的`models.py`定义了三组模型，分别为versioned models、nested versioned models、multi part versioned models。第一类是基础模型，第二类是课程数据模型的抽象，第三类是商品数据模型的抽象。

## 设计实现方式

什么样的实现方式是好的呢？

在纷繁复杂的具体情况找到共同点，建立尽可能简单且普世的模型解决问题。以`drf-versioned-models`为例，选择的方法是把模型分为不可变的主体和可变的版本化两个模型，再通过一个序列化类处理他们。这样最大限度地利用现有Django和DRF库的实现，并且核心代码相对较少且较为简单。

更简洁意味着更容易维护。注意抽象可以复用的部分，比如验证逻辑、异常逻辑等。
