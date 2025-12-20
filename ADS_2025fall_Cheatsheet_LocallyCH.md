# 1 二分查找
## 1.1 一类二分查找问题的通法

不失一般性，我们假设：$key(arr[idx])$ 关于 $idx$ 单调增加。否则，若单调减少，取负的函数值即可。

### 1.1.1 寻找arr中从左往右第一个使得key函数值>x（或>=x）的元素

直接参考bisect库源码即可。注意“循环不变量”的思想：要保证目标下标始终位于闭区间 $[lo, hi]$ 中，这样更新边界的时候就不会搞错“加不加1”“减不减1”这类问题。

```python
#寻找第一个使key值>x的元素，返回a中下标
#若无满足条件的下标，返回len(a)
def bisect_strict_first(a, key):
	lo, hi = 0, len(a)
	while lo < hi:
		mid = (lo + hi) // 2
		if key(a[mid]) > x:
			hi = mid
		else:
			lo = mid + 1
    return lo

#寻找第一个使key值>=x的元素，返回a中下标
#若无满足条件的下标，返回len(a)
def bisect_nonstrict_first(a, key):
	lo, hi = 0, len(a)
	while lo < hi:
		mid = (lo + hi) // 2
		if key(a[mid]) < x:
			lo = mid + 1
		else:
			hi = mid
	return lo
```

==注意：== 该源码实际上可以用于寻找合适的插入x的位置，即使数组中元素全部<=x（或全部<x），此代码仍然有效。初始化 $hi$ 为 $len(a)$ 保证了这一点。

稍加修改，我们就能得到另一类镜像问题的解法：

### 1.1.2 寻找arr中从左往右最后一个使得key函数值<x（或<=x）的元素

```python
#寻找最后一个使key值<x的元素，返回a中下标
#若无满足条件的下标，返回-1
def bisect_strict_last(a, key):
	lo, hi = -1, len(a) - 1
	while lo < hi:
		mid = (lo + hi + 1) // 2
		if key(a[mid]) < x:
			lo = mid
		else:
			hi = mid - 1
	return hi
	
#寻找最后一个使key值《=x的元素，返回a中下标
#若无满足条件的下标，返回-1
def bisect_nonstrict_last(a, key):
	lo, hi = -1, len(a) - 1
	while lo < hi:
		mid = (lo + hi + 1) // 2
		if key(a[mid]) > x:
			hi = mid - 1
		else:
			lo = mid
	return hi
```