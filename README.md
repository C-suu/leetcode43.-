# leetcode43. 接雨水 给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

<img width="1794" height="798" alt="image" src="https://github.com/user-attachments/assets/f123f13d-5308-40bc-b440-3f8ae4c77c18" />

这是 **LeetCode 第 42 题：接雨水（Trapping Rain Water）**。
会 **一步一步来**，**非常偏小白视角**，做到：

1️⃣ 先给出 **最终可提交的答案代码**
2️⃣ 再 **逐字母、逐行解释代码在干什么**
3️⃣ 最后给出一个 **和代码完全对应的表格示例推演**（能对着代码看表）

---

## 一、最终答案（推荐解法：双指针，O(n)，面试最爱）

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
class Solution:
```

* LeetCode 固定写法
* `Solution` 是一个类

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
