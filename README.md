# leetcode42. 接雨水 给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

## 注意，比如一个格子能存2格水，但是这个格子有一个柱子，那就变成了2-1=1，只能存1格水。柱子也占一个位置。这个需要注意。

<img width="1794" height="798" alt="image" src="https://github.com/user-attachments/assets/f123f13d-5308-40bc-b440-3f8ae4c77c18" />

这是 **LeetCode 第 42 题：接雨水（Trapping Rain Water）**。
会 **一步一步来**，**非常偏小白视角**，做到：

1️⃣ 先给出 **最终可提交的答案代码**
2️⃣ 再 **逐字母、逐行解释代码在干什么**
3️⃣ 最后给出一个 **和代码完全对应的表格示例推演**（能对着代码看表）

---

## 一、最终答案（推荐解法：双指针，O(n)）

```python
class Solution:
    def trap(self, height):
        left = 0                          # 左指针，从最左边开始
        right = len(height) - 1           # 右指针，从最右边开始
        left_max = 0                      # 左侧目前遇到的最高柱子
        right_max = 0                     # 右侧目前遇到的最高柱子
        water = 0                         # 累计接到的雨水总量

        while left < right:               # 左右指针未相遇时持续计算
            if height[left] < height[right]:   # 左边柱子更矮，先处理左边
                if height[left] >= left_max:   # 当前左柱 >= 左侧最高
                    left_max = height[left]    # 更新左侧最高柱子
                else:
                    water += left_max - height[left]  # 左侧最高 - 当前高度 = 水量
                left += 1                      # 左指针右移
            else:                              # 右边柱子更矮或相等，处理右边
                if height[right] >= right_max: # 当前右柱 >= 右侧最高
                    right_max = height[right]  # 更新右侧最高柱子
                else:
                    water += right_max - height[right] # 右侧最高 - 当前高度 = 水量
                right -= 1                     # 右指针左移

        return water                           # 返回最终雨水总量
```

### 一句话记忆版（配合右侧注释看）

> **谁矮算谁，用“历史最高 − 当前高度”加水，然后指针向中间走。**

---

## 二、先用一句「人话」理解这题在干嘛

👉 **每一根柱子能装多少水 =
它左边最高的柱子 和 右边最高的柱子 中的较小值 − 它自己的高度**

也就是公式：

```
water[i] = min(left_max[i], right_max[i]) - height[i]
```

但们 **不真的存数组**，而是用 **双指针实时算**。

---

## 三、逐字母 + 逐行解释代码（超详细）

### 1️⃣ 函数定义

```python
def trap(self, height):
```

* `trap`：函数名
* `height`：题目给的柱子高度数组，比如 `[0,1,0,2,...]`

---

### 2️⃣ 初始化变量（非常重要）

```python
left = 0
```

* `left`：左指针
* 一开始指向 **数组最左边**

```python
right = len(height) - 1
```

* `right`：右指针
* 指向 **数组最右边**

```python
left_max = 0
```

* `left_max`：从左边走过来见过的 **最高柱子**

```python
right_max = 0
```

* `right_max`：从右边走过来见过的 **最高柱子**

```python
water = 0
```

* `water`：最终能接到的雨水总量

---

### 3️⃣ 核心循环（左右夹逼）

```python
while left < right:
```

* 只要左右指针 **还没相遇**
* 就说明中间 **还有柱子没算**

---

### 4️⃣ 为什么要比较左右高度？

```python
if height[left] < height[right]:
```

👉 **决定权在短的一边**

* 如果左边更低
  → 左边能装多少水，只取决于 `left_max`
* 因为右边一定能兜住水（更高）

---

### 5️⃣ 处理左边柱子（高度较小）

```python
if height[left] >= left_max:
```

* 如果当前柱子比历史最高还高
* ❌ 装不了水
* ✅ 更新左边最高

```python
left_max = height[left]
```

---

```python
else:
    water += left_max - height[left]
```

* 如果当前柱子 **低于 left_max**
* 那它头顶可以装水
* 装的水量 = `left_max - 当前高度`

---

```python
left += 1
```

* 左指针右移
* 去算下一个柱子

---

### 6️⃣ 处理右边柱子（对称逻辑）

```python
else:
```

* 说明 `height[right] <= height[left]`
* 那就处理右边

---

```python
if height[right] >= right_max:
```

* 当前柱子比右侧最高还高
* 更新右侧最高

```python
right_max = height[right]
```

---

```python
else:
    water += right_max - height[right]
```

* 当前柱子比 `right_max` 低
* 可以装水

---

```python
right -= 1
```

* 右指针左移

---

### 7️⃣ 循环结束，返回答案

```python
return water
```

* 返回接到的总雨水量

---

## 四、表格示例推演（和代码一一对应）

### 示例数组

```text
height = [0,1,0,2,1,0,1,3,2,1,2,1]
```

---

### 代码运行过程表格（关键步骤）

| left | right | height[left] | height[right] | left_max | right_max | 新增水量 | 总水量   |
| ---- | ----- | ------------ | ------------- | -------- | --------- | ---- | ----- |
| 0    | 11    | 0            | 1             | 0        | 0         | 0    | 0     |
| 1    | 11    | 1            | 1             | 1        | 0         | 0    | 0     |
| 2    | 11    | 0            | 1             | 1        | 0         | 1    | 1     |
| 3    | 11    | 2            | 1             | 1        | 0         | 0    | 1     |
| 3    | 10    | 2            | 2             | 1        | 2         | 0    | 1     |
| 4    | 10    | 1            | 2             | 1        | 2         | 0    | 1     |
| 5    | 10    | 0            | 2             | 1        | 2         | 1    | 2     |
| 6    | 10    | 1            | 2             | 1        | 2         | 0    | 2     |
| 7    | 10    | 3            | 2             | 1        | 2         | 0    | 2     |
| 7    | 9     | 3            | 1             | 1        | 2         | 1    | 3     |
| 7    | 8     | 3            | 2             | 1        | 2         | 0    | 3     |
| 7    | 7     | ——           | ——            | ——       | ——        | ——   | **6** |

👉 **最终答案：6**

---

## 五、为什么这个方法这么好？

* ✅ 时间复杂度：`O(n)`
* ✅ 空间复杂度：`O(1)`
* ✅ 不用额外数组
---

# 一个很重要的问题：
```
            if height[left] < height[right]:   # 左边柱子更矮，先处理左边
                if height[left] >= left_max:   # 当前左柱 >= 左侧最高
                    left_max = height[left]    # 更新左侧最高柱子
                else:
                    water += left_max - height[left]  # 左侧最高 - 当前高度 = 水量
                left += 1                      # 左指针右移


对于这个循环，不是很理解。“if height[left] < height[right]:”理解，但是下面的:

                “if height[left] >= left_max:   # 当前左柱 >= 左侧最高
                    left_max = height[left]    # 更新左侧最高柱子
                else:
                    water += left_max - height[left]  # 左侧最高 - 当前高度 = 水量
                left += 1                      # 左指针右移”
啥意思？if 当前左边高度大于最大左边高度，然后更新左边最大高度，这个没问题。

但是什么叫if 当前左边高度不大于最大左边高度，即“else”后面的操作，直接计算water += left_max - height[left] ？

而且这个+=后面为何还是一个减法运算？

首先重复叙述的问题。在理解的基础上，把这段代码掰开揉碎讲清楚。
```

---

## 一、先重复的问题（非常重要）

现在的困惑是：

> 能理解
> `if height[left] < height[right]:`
> ——也就是“左边更矮，先处理左边”。

但是**不理解下面这一段在干什么**：

```python
if height[left] >= left_max:
    left_max = height[left]
else:
    water += left_max - height[left]
left += 1
```

具体疑问是三点：

1️⃣ **为什么**

> “如果当前左边高度不大于 `left_max`，就可以直接加水？”

2️⃣ **为什么加的水是**

> `left_max - height[left]`
> 而且还是一个**减法**？

3️⃣ **为什么可以当场算水**，而不用再等右边的情况？

——对不对？
如果对，下面正式开始解释。

---

## 二、先给一句“核心结论”（先立法，再讲理）

> **当 `height[left] < height[right]` 成立时：
> 当前左边柱子“能装多少水”，已经完全确定了，
> 右边不会再影响它了。**

这是整段逻辑的**灵魂**。
现在不理解，是因为这句话还没“信”。

下面一步一步让**被迫相信它**。

---

## 三、为什么 `height[left] < height[right]` 时，只看左边？

### 关键物理事实（不是算法）

**一根柱子能接水，必须满足：**

* 左边有挡板
* 右边也有挡板
* 水位高度 = 左右挡板中 **较矮的那一个**

也就是：

```
水位 = min(左边最高, 右边最高)
```

---

### 现在看这一刻的状态

```python
height[left] < height[right]
```

说明什么？

👉 **右边此刻至少有一根柱子，比左边当前柱子高**

哪怕右边以后再怎么变：

* 右边“最低保障高度” ≥ `height[right]`
* 而 `height[right]` 已经 **高于当前左柱**

✅ 所以：
**左边这根柱子，右侧挡板一定是“够用的”**

> 它不会是限制因素
> 真正限制水位的，只可能是左边

---

## 四、现在正式拆不懂的那段代码

### 情况 1：当前左柱 ≥ left_max

```python
if height[left] >= left_max:
    left_max = height[left]
```

#### 这代表什么现实情况？

* 走到了一根**新的更高的左挡板**
* 它本身是“墙”，不是“坑”

比如：

```
left_max = 3
当前 height[left] = 4
```

这根柱子：

* 自己比水位还高
* **不可能存水**

所以只做一件事：

👉 **更新左侧最高挡板**

---

### 情况 2（重点）：当前左柱 < left_max

```python
else:
    water += left_max - height[left]
```

真正卡在这里，对吧？
们慢慢来。

---

## 五、为什么“当前左柱 < left_max”就一定能存水？

因为此时 **三件事同时成立**：

### ① 左边有更高的挡板

```python
left_max > height[left]
```

### ② 右边一定有挡板（刚才证明过）

```python
height[right] > height[left]
```

### ③ 所以左右挡板都齐了 ✅

这根柱子**已经被左右包死了**，不会漏水。

---

## 六、那水到底能装多高？

现在关键来了：

### 水位高度 = min(左挡板, 右挡板)

但现在处在这个分支：

```python
height[left] < height[right]
```

说明：

```
右挡板 ≥ height[right] > height[left]
```

而左挡板高度是：

```
left_max
```

于是：

```
水位 = min(left_max, 右挡板) = left_max
```

**右边再高都没用，短板是左边。**

---

## 七、终于解释最困惑的那一行

```python
water += left_max - height[left]
```

### 这是怎么来的？

* 水能灌到高度：`left_max`
* 当前柱子本身高度：`height[left]`

所以：

```
水的高度 = left_max - height[left]
```

这是**纯几何高度差**，不是拍脑袋。

---

## 八、为什么可以“直接加水”，不用等等右边？

因为此刻已经满足：

* 左挡板高度：已知（left_max）
* 右挡板高度：**至少比当前柱子高**

> 不管以后右指针怎么移动，
> 都不会让这根柱子“漏水”或“多装水”。

所以：

👉 **现在算，和将来算，结果一模一样**
👉 那就现在算，效率最高

---

## 九、为什么算完立刻 `left += 1`？

```python
left += 1
```

这表示：

> **这根柱子的一生已经结束了**
> 水算完了，再也不用回头看它了

这是双指针算法的精髓：
**每个位置只算一次，绝不回头**

---

## 十、用一句“人话”总结卡住的地方

> 当 `height[left] < height[right]` 时：
> 当前左柱子的“右挡板”已经确定是够高的，
> 能装多少水只取决于左边最高的挡板。
>
> 如果它比左挡板矮，
> 那 `left_max - height[left]` 就是它头顶能装的水。

---

# 四个数的例子

好，这次们**彻底简化**，
不用 12 个数，不用长表，
**只用 4 个柱子**，而且**只看刚才卡住的那段代码在干什么**。

---

## 一、极简示例数组（4 个数）

们用这个数组：

```text
height = [3, 0, 2, 4]
```

画成柱子是这样（高度很直观）：

```
位置:   0   1   2   3
高度:   3   0   2   4
```

---

## 二、们只关注这一段代码（左边分支）

```python
if height[left] < height[right]:
    if height[left] >= left_max:
        left_max = height[left]
    else:
        water += left_max - height[left]
    left += 1
```

---

## 三、代码运行过程【简化表格版】

### 初始状态

* `left = 0`
* `right = 3`
* `left_max = 0`
* `water = 0`

---

### 第 1 步

| left | right | height[left] | height[right] | left_max | 新增水量 | 总水量 |
| ---- | ----- | ------------ | ------------- | -------- | ---- | --- |
| 0    | 3     | 3            | 4             | 0        | 0    | 0   |

**解释：**

* `height[left] < height[right]` → `3 < 4` ✅
* `height[left] >= left_max` → `3 >= 0` ✅
* 更新：`left_max = 3`
* 没有水（这是挡板）
* `left → 1`

---

### 第 2 步（重点来了）

| left | right | height[left] | height[right] | left_max | 新增水量  | 总水量 |
| ---- | ----- | ------------ | ------------- | -------- | ----- | --- |
| 1    | 3     | 0            | 4             | 3        | **3** | 3   |

**解释（非常关键）：**

* `0 < 4` ✅ → 继续算左边
* `height[left] >= left_max` → `0 >= 3` ❌
* 进入 `else`

```python
water += left_max - height[left]
       = 3 - 0
       = 3
```

📌 **物理意义**：

* 左挡板高度 = 3

* 右挡板至少 = 4（够高）

* 当前柱子高度 = 0
  ➡️ 上面可以灌 **3 格水**

* `left → 2`

---

### 第 3 步

| left | right | height[left] | height[right] | left_max | 新增水量  | 总水量 |
| ---- | ----- | ------------ | ------------- | -------- | ----- | --- |
| 2    | 3     | 2            | 4             | 3        | **1** | 4   |

**解释：**

* `2 < 4` ✅
* `2 >= 3` ❌
* 继续存水：

```python
water += 3 - 2 = 1
```

📌 这一格上面只能存 **1 格水**

* `left → 3`
* `left == right`，结束

---

## 四、最终结果

```text
总水量 = 4
```

---

## 五、把最困惑的问题用一句话说清楚

> **为什么 `water += left_max - height[left]` 是减法？**

因为：

* 水能到的高度是 `left_max`
* 柱子自己已经占了 `height[left]`
* **剩下的空间，才是水**

就像：

```
水深 = 水面高度 − 地面高度
```

---

## 六、一句“彻底想通版”

> 当 `height[left] < height[right]` 时，
> 当前左柱子的右边一定兜得住水，
> 能装多少水只看左边最高的挡板，
> 所以直接用
> `left_max - height[left]`，
> 现在算、以后算，结果都一样。

---

