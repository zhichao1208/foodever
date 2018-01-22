# 数据类型

## 菜品

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

## 食材 （暂缺）

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
- 可做菜品
  - 西红柿炒鸡蛋
  - 番茄牛腩
  - 番茄土豆
  - ...
          
状态：
- 可选食材：默认状态 
- 已选食材：用户选择的现在可以使用的食材 
- 缺少食材：某一个包含用户已选食材的菜品中的其他**主料**，即用户未选的食材，认为是用户缺少的食材 （菜品选择页）
- 待购食材：当用户完成菜品选择后，最终生成的菜单中，仍然缺少的食材

# 算法说明

- 已选食材：X
- 菜品主料：Y
- 已选食材与菜品主料的交集：N
- 主料-交集（缺少的食材）：  Z

## 判断条件：N>0 （这些菜品的主料中包括至少一种已选食材）

- ### 第一分类：Z=0 （这些菜品的主料全部包含于已选食材：可以直接做）
- 展示规则：全部展示符合条件的菜品
- 排序优先级：
1. 按时间排序 （所需时间越短，越靠前）
2. 按照所需主料数量排序 （所需主料数量越少，越靠前）

- ### 第二分类：Z=1 （这些菜品的主料只差一种就可以做）
- 展示规则：全部展示符合条件的菜品
- 排序优先级：
1. 按时间排序 （所需时间越短，越靠前）
2. 按照所需主料数量排序 （所需主料数量越少，越靠前）

- ### 第三分类：Z>1 （这些菜品的主料差不止一种食材才可以做）
- 展示规则：20 - 前两个类别 （仅当前两种类别不到20个时，才展示第三分类且仅补足20个）
- 排序优先级：
1. 按时间排序 （所需时间越短，越靠前）
2. 按照所需主料数量排序 （所需主料数量越少，越靠前）

## 判断条件：N=0 （这些菜品的主料中不包括任何一种已选食材）
- #### 第一分类：X=1 （展示主料只有一种的菜品）
- 展示规则：每种类别排序为0-1的主料展示前2个菜
- 排序优先级：
1. 按时间排序 （所需时间越短，越靠前）

# 页面说明

## 整体布局

- 流程：**食材选择页** -> **菜品选择页** -> **结果页**
- 详情页：
  - **菜品详情页**
  - 食材详情页（暂缓）。
- 分享弹窗：
  - 主流平台
  - 保存页面截图到相册。

## 食材选择页 （首页）
### 板块：
- 标题
- 食材分类列表 
   - 食材栏
- 底栏

### 标题：
|     数据     | 内容 |    备注  |
| ------------ | --- | ---------------|
| 标题 |  你现在有什么材料？ |  |

### 食材分类列表（每个分类默认仅展示排序为0-4的前5种食材）

|     数据     | 内容 |    备注  |
| ------------ | --- | ---------------|
| 类别 |  食材所属类别名称 |  |
| 食材栏1 |  单个食材图片；名称；选框 |  鸡蛋；西红柿为默认勾选，其他为默认不选 |
| 食材栏2 |  单个食材图片；名称；选框 |  同上 |
| 食材栏3 |  单个食材图片；名称；选框 |  同上 |
| 食材栏4 |  单个食材图片；名称；选框 |  同上 |
| 食材栏5 |  单个食材图片；名称；选框 |  同上 |
| 常用 |  “查看更多” | 序号为0-4的食材，点击切换到 **“全部”** |
| 全部 |  “收起” | 该类中的全部食材，点击切换到 **“常用”** |
 
### 底栏
已选食材数：X
可选菜品数：Y （*主料-交集即缺少的食材Z=0时，**全部主料都被包含于已选食材中的菜品总数** *）

|     数据     | 内容 | 备注  |
| ------------ | --- | ---------------|
| 文字1 | 已选X个食材 | 灰色 |
| 文字2 |  可选Y道菜 | 灰色 |
| 按钮 | 下一步 | X>0 高亮，点击进入 **【菜品选择页】** ；X=0，灰色不可选。 |

### 状态：
默认：
- 已选食材：鸡蛋；西红柿。
- 所有食材分类：收起 （每个分类仅展示排序为0-4的前5种食材）

### 例子： 
- 默认已选
  - 鸡蛋
  - 西红柿。
- 底栏显示：
  - 文字1: 已选2个食材
  - 文字2: 可做20道菜 （Y=20）
  - 按钮：下一步（高亮，点击进入 **【菜品选择页】**）
  
------

## 菜品选择页 （第二步）

### 板块
- 顶栏
- 可选菜品列表
- 底栏

### 顶栏
|     数据     | 内容 | 备注  |
| ------------ | --- | ---------------|
| 返回 | 返回图标 | 点击回到**【食材选择页】** |
| 标题 | 看看想做什么菜 | |

### 可选菜品列表

展示规则：*参见 算法说明*

#### 列表展示分类：
- 类别1: 缺少食材为0的菜品列表
- 类别2: 缺少食材为1的菜品列表
- 类别3: 当前两种数量少于20时，补充缺少食材超过1种的菜品列表至20。

#### 单位：单个菜品框

|     数据      | 内容 | 备注   |
| --------------  | ---  | ---------------|
| 图片 | 菜品图片 | 点击进入**【菜品详情页】** |
| 时间 | 菜品图片上的悬浮标签 |  |
| 标题 | 菜品名称 | 如缺少食材为0，则上下居中。如缺少食材不为0，则靠上显示 |
| 缺少食材 | “还需： [缺少食材文字]” | 缺少食材不为0则显示，最多10个字符，折1行，超出直接切断；缺少食材为0则不显示|
| 按钮：添加 | 默认 “+” | 添加菜品到**【结果页】**；如缺少食材不为0，则添加缺少食材至**【结果页-待够清单】**|
| 按钮：已选 | 已选状态 “对勾” | 把菜品从**【结果页】**删除；如缺少食材不为0，则将缺少食材从**【结果页-待够清单】**中删除|

点击区域备注：
- 图片+时间 -> **【菜品详情页】**
- 标题+按钮（+缺少食材） -> 操作 （编辑结果及待够清单）

### 底栏

已选菜品：A
预计用时：M （所有已选菜品的所需时间之和 - 可微调减少5-10分钟共同备菜操作时间）

|     数据     | 内容 | 备注  |
| ------------ | --- | ---------------|
| 文字1 | 已选A道菜 | 灰色 |
| 文字2 | 预计用时M分钟 | 灰色 |
| 按钮 | 下一步 | A>0 高亮，点击进入 **【结果页】** ；A=0，灰色不可选。 |

#### 例子：
已选菜品：西红柿炒鸡蛋 5分钟；番茄牛腩 20分钟。
预估用时：预计用时25分钟
按钮：下一步

------

## 结果页 （第三步）


### 板块
- 顶栏
- 结果列表
  - 菜品列表
    - 编辑状态
  - 待购清单
- 底栏

### 顶栏

|     数据     | 内容 | 备注  |
| ------------ | --- | ---------------|
| 返回 | 返回图标 | 点击回到**【菜品选择页】** |
| 标题 | 今日菜谱 | |
| 编辑 | 编辑图标 | 点击进入**“编辑状态”** |

### 结果列表

菜品列表 > 单个菜品框

|     数据      | 内容 | 备注  |
| --------------  | ---  | ---------------|
| 图片 | 菜品图片 | 点击进入**【菜品详情页】** |
| 时间 | 菜品图片上的悬浮标签 |  |
| 标题 | 菜品名称 | 上下居中 |

#### 编辑状态：
- 点击编辑图标：退出／进入编辑状态
  - 列表进入可编辑状态，晃动
  - 列表整体右移
  - 每个结果左侧出现“删除图标”

#### 删除操作：编辑状态中点击“删除图标”

判断条件
1. 如该菜品包括的**缺少食材**数>0
- 该结果消失
- **替代文字**出现：“菜品名称”已删除， *点此撤销*
- 如果某缺少食材为此菜品独有，则删除【结果页-待购清单】中相应的**缺少食材**
  1.1 判断：如果某缺少食材并非此菜品独有，则更新该缺少食材的所属菜品名称

2. 如该菜品不包括**缺少食材**，
- 该结果消失
- **替代文字**出现：“菜品名称”已删除， *点此撤销*

其他操作
- 点击撤销：结果恢复
- 点击其他删除图标：出现相应的**替代文字**
- 点击编辑图标：退出编辑状态，**替代文字**和删除图标消失
- 不操作：再次刷新，或重新进入页面时和退出编辑状态时，**替代文字消失**
- 重新刷新或进入页面：退出编辑状态

### 待购清单
食材列表 > 单个食材栏

|     数据      | 内容 | 备注  |
| --------------  | ---  | ---------------|
| 图片 | 食材图片 |  |
| 名称 | 食材名 |  |
| 所属菜品 | 菜品名称 | 灰色小字。所属菜品=1，则显示菜品名。所属菜品>1,显示最后添加的菜品名+“等” |

更新条件
- 菜品结果列表有变化，则更新待购清单。及清单内食材中的所属菜品显示。

### 底栏

已选菜品：A
预估用时：M （所有已选菜品的所需时间之和 - 可微调减少5-10分钟共同备菜操作时间）

|     数据     | 内容 | 备注  |
| ------------ | --- | ---------------|
| 文字1 | 已选A道菜 | 灰色 |
| 文字2 | 预计用时M分钟 | 灰色 |
| 按钮 | 保存 | A>0 高亮，点击进入 **【分享弹窗】** ；A=0，灰色不可选。 |

### 分享弹窗

|     数据     | 内容 | 备注  |
| ------------ | --- | ---------------|
| 分享 | 主流平台图标 |  |
| 保存图片 | 图标 | 保存【结果页】截图到系统相册 |
| 保存到清单 | 图标 | 保存待购清单到系统清单 |


