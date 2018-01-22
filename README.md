# 算法说明
## 数据类型

### 菜品

|     数据     | 内容 |    备注  |
| ------------ | --- | ---------------|
| 名称 |  菜品名称 |  |
| ID |  菜品ID |  |
| 图片 |  菜品图片 |  |
| 主料 |  必要的食材 | 最多5种，最少1种 |
| 配料 |  其他不必要的食材 | 最多20种，最少0种 |
| 步骤 |  1.图片 2.序号 3.内容 |  |
| 时间 |  所需时间 | 单位统一为分钟 |

例子：
- 名称：西红柿炒鸡蛋
- ID： 123123
- 图片：XXX.jpg
- 主料：番茄；鸡蛋
- 配料：葱花；姜；番茄酱
- 步骤：
 - 1.1 图片：XXX.jpg；
 - 1.2 序号:1；
 - 1.3 内容：热油锅，下葱花和姜；
      ...
- 时间：5分钟

状态：
- 可选菜品：菜品默认状态 
- 已选菜品：用户在本地选择准备做的菜品

### 食材

|     数据     | 内容 |             备注                |
| ------------ | --- | ------------------------------- |
| 名称 |  食材名称 |  |
| ID |  食材ID |  |
| 图片 | 食材图片 |  |
| 类型 | 食材所属类型 |  |
| 序号 | 食材在所属类型中的排序 | 默认类型收起状态展示0-4 |
| 可做菜品 |  包含此食材的菜品数据表 |  |

例子：
- 名称：番茄
- ID： 123123
- 图片：XXX.jpg
- 类型：蔬菜
- 序号：2
- 可做菜品：西红柿炒鸡蛋；
          番茄牛腩；
          番茄土豆；
          ...
          
状态：
- 可选食材：默认状态 
- 已选食材：用户选择的现在可以使用的食材 
- 缺少食材：某一个包含用户已选食材的菜品中的其他**主料**，即用户未选的食材，认为是用户缺少的食材 （菜品选择页）
- 待购食材：当用户完成菜品选择后，最终生成的菜单中，仍然缺少的食材

## 算法说明

- 已选食材：X
- 菜品主料：Y
- 已选食材与菜品主料的交集：N
- 主料-交集（缺少的食材）：  Z

1. N>0
|
|--- Z=0






