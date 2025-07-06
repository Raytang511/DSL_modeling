# 实验三：特定领域建模实验报告（BPMN）

## 1. 项目概述

本项目基于 Eclipse 建模框架，围绕 **BPMN（Business Process Model and Notation）** 构建了一个特定领域建模语言，涵盖了以下内容：

- 使用 **Eclipse EMF** 进行 BPMN 元模型建模
- 使用 **Eclipse Sirius** 进行 BPMN 图形语言建模
- 使用 **Eclipse Xtext** 实现 BPMN 文本语言建模
- 构建插件并在 Eclipse 中进行测试

本实验意在加深对模型驱动工程（Model-Driven Engineering, MDE）的理解与实践能力。

## 2. 项目链接

> GitHub 项目地址  
> https://github.com/Raytang511/DSL_modeling.git

## 3. 目标模型简介：BPMN

**BPMN（Business Process Model and Notation）** 是一种用于业务流程建模的图形化表示标准，它通过流程图的方式来描述业务流程，帮助业务分析师、开发人员与管理人员在一个统一的视图中交流。

BPMN 的核心元素包括：

- **流程（Process）**：由任务、事件、网关等组成。
- **任务（Task）**：可执行的最小单元。
- **事件（Event）**：流程的触发器或终点（如开始、结束事件）。
- **网关（Gateway）**：流程的分支或合并节点。

## 4. Eclipse EMF 元模型建模

### 4.1 元模型结构说明

在 Eclipse EMF 中，我们根据 BPMN 的基本组成构建了如下核心类：

- `Process`：业务流程，包含多个任务和流程控制逻辑
- `Task`：单个可执行任务，拥有名称、负责人等属性
- `Event`：流程起止点，分为开始事件和结束事件
- `Gateway`：用于控制流程的条件分支

这些类之间通过 `EReference` 进行关联，以表达流程中元素的组合关系。



## 5. Eclipse Sirius 图形语言建模

### 5.1 图形建模配置说明

基于 EMF 元模型，配置 Sirius 图形语言支持：

- 创建 Viewpoint Specification Project
- 配置 Diagram 表示：BPMNDiagram
- 定义映射关系（Mapping）：
  - `NodeMapping`：映射 Task、Event、Gateway
  - `EdgeMapping`：表示流程顺序流（Sequence Flow）
- 自定义样式，例如不同图标表示不同类型的事件



## 6. Eclipse Xtext 文本语言建模

### 6.1 语法规则说明

在 EMF 元模型基础上，借助 RM2PT 工具自动生成 Xtext 语法文件。对语法进行了简化和调整：

```antlr
Process:
  'process' name=ID '{'
    (tasks+=Task | events+=Event | gateways+=Gateway)*
  '}';

Task:
  'task' name=ID 'assignedTo' assignee=ID;

Event:
  'startEvent' name=ID | 'endEvent' name=ID;

Gateway:
  'gateway' name=ID 'type' type=GatewayType;

enum GatewayType:
  XOR | AND | OR;
```

### **6.2 编辑器截图**

## **7. 插件构建及测试**

### **7.1 插件构建说明**

插件构建步骤：

- 创建 Feature Project，包含 DSL 项目
- 创建 Update Site Project，用于打包发布
- 使用 Export Deployable Features 导出为 Eclipse 插件
- 在 Eclipse 新环境中导入插件，验证可用性



## **8. 结论与收获**

通过本次实验，我掌握了基于 Eclipse 平台的 DSL 构建流程，从元模型设计到图形与文本语言开发，再到插件打包部署，完整实现了对 BPMN 模型的建模支持。



**主要收获包括：**

- 熟悉了 EMF/Sirius/Xtext 工具链
- 理解了模型驱动工程的核心思想
- 提升了 Eclipse 插件开发和集成能力

